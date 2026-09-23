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
