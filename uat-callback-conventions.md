# Wireshark UAT Callback Conventions

This file records durable callback and persistence conventions for Wireshark User Accessible Tables (UATs). Current upstream `epan/uat.h` and UAT implementations remain authoritative.

## Record transformations must survive the copy/save lifecycle

A UAT `update_cb` is primarily a validation hook. Mutating the supplied record there is unsafe as a persistence mechanism unless the same transformation is also applied to newly copied records, because a later save can operate on the copied representation and lose update-only changes.

Merged MR !12967, authored and merged by John Thacker, documents the supported exception explicitly: an update callback may transform a record if the copy callback performs the same transformation, for example by having `copy_cb` invoke `update_cb` on the new record. The comment also notes that a validation-only update callback would conceptually be clearer with a const record pointer.

**Implementation rule:** keep UAT validation and normalization separate when practical. If normalization is performed in `update_cb`, make it part of record construction/copy as well so the in-memory representation and later serialized representation converge on the same canonical value.

**Review rule:** for a UAT callback that mutates its record, trace edit, copy, reload, and save paths. Do not verify only that the current dialog display looks correct; verify that the transformed value remains correct after the record is copied and serialized again.

**Confidence:** Very high for the API contract. The clarification was authored and merged directly by John Thacker in `uat.h`.

## Use `post_update_cb` for derived state that must be rebuilt after every table change

A UAT `update_cb` is not a general notification that runs after every persistent table update. It validates the copy placed in `user_data`; after saving, previously validated `raw_data` records can be copied back into `user_data` without another `update_cb` call. Derived state attached only by `update_cb` can therefore silently disappear after a save.

Merged master MR !12881, authored by John Thacker and merged by Anders Broman, fixes display-filter macros that were parsed into parts and argument positions on initial load but could lose that derived representation after the UAT was saved. The accepted change makes the update callback validation-only and reparses every macro from `post_update_cb`, the callback that is guaranteed to run when the table data has changed.

**Implementation rule:** use field checks and `update_cb` for validation. Use `post_update_cb` to rebuild caches, parsed representations, indexes, or other table-wide derived state that must be regenerated whenever the accepted UAT contents change. Do not depend on `update_cb` as an all-path change notification.

**Review rule:** for UAT-backed derived state, test at least initial load, edit/accept, save, and subsequent use. A callback arrangement that works immediately after editing can still be wrong after the save/copy lifecycle reconstructs `user_data`.

**Confidence:** Very high. The lifecycle distinction and failure mode are stated directly in a merged master fix authored by John Thacker, and it sharpens the copy/save rule established by !12967.

## Store UAT values in the semantic type exposed by the schema

A UAT field that is intrinsically numeric should normally use numeric storage and the matching UAT field callback instead of storing text and adding manual parse, copy, validation, and free logic.

In merged MR !7468, DNS server ports were initially represented as strings. Jaap Keuter explicitly asked why they were not using `UAT_DEC_CB_DEF`. Merged follow-up !7471 implements that review: TCP and UDP ports become integer fields with decimal UAT callbacks, removing the string ownership and conversion helpers.

**Implementation rule:** choose UAT storage/callback types from the configuration value's semantic domain. Use strings for textual data, not merely because the editor ultimately displays text.

**Confidence:** Very high. Direct Jaap Keuter review followed by a merged corrective MR.


## Free UAT-derived state according to its full ownership graph

Merged MR !7088 fixes SOME/IP configuration cleanup by replacing generic outer-object destruction with type-specific destructors that also release nested arrays and other owned allocations. Its record free callbacks clear released string pointers. Merged !7079 independently extends the Signal-PDU UAT free callback to release every owned string and clear the corresponding fields.

**Implementation rule:** a UAT or derived-cache destructor must mirror the actual ownership graph. If a hash value owns child arrays or strings, free those children before the outer value rather than relying on a generic shallow destructor. When a record object survives a cleanup callback, leave released owned pointers in a defined safe state.

**Review rule:** when adding fields to a UAT record or derived cache object, update copy, free, and reset lifecycle code at the same time and audit every ownership-bearing member.

**Confidence:** High. Two merged configuration-memory fixes, with detailed review on the SOME/IP cleanup.


## Set callbacks can receive transient invalid values before check callbacks

Merged master MR 4905, authored by John Thacker, fixes an EPL UAT overflow found by ASAN. The Qt UAT model calls the field setter with an empty string while inserting a new row before the check callback validates the value. The accepted implementation makes the setter tolerant of invalid input and uses the same `hex_str_to_bytes()` interpretation in both setter and validator.

**Callback-order rule:** a UAT field setter must not assume that the check callback already accepted its input. Treat empty and partially edited text as normal transient states, leave the record in a safe placeholder state when parsing fails, and let validation produce the user-facing error.

**Consistency rule:** where the setter and validator both parse the same textual representation, share the same parsing primitive and acceptance criteria.

**Confidence:** Very high. Merged master correctness fix by John Thacker with an ASAN-detected concrete failure mode.
