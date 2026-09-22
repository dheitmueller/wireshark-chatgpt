# Wireshark UAT Callback Conventions

This file records durable callback and persistence conventions for Wireshark User Accessible Tables (UATs). Current upstream `epan/uat.h` and UAT implementations remain authoritative.

## Record transformations must survive the copy/save lifecycle

A UAT `update_cb` is primarily a validation hook. Mutating the supplied record there is unsafe as a persistence mechanism unless the same transformation is also applied to newly copied records, because a later save can operate on the copied representation and lose update-only changes.

Merged MR !12967, authored and merged by John Thacker, documents the supported exception explicitly: an update callback may transform a record if the copy callback performs the same transformation, for example by having `copy_cb` invoke `update_cb` on the new record. The comment also notes that a validation-only update callback would conceptually be clearer with a const record pointer.

**Implementation rule:** keep UAT validation and normalization separate when practical. If normalization is performed in `update_cb`, make it part of record construction/copy as well so the in-memory representation and later serialized representation converge on the same canonical value.

**Review rule:** for a UAT callback that mutates its record, trace edit, copy, reload, and save paths. Do not verify only that the current dialog display looks correct; verify that the transformed value remains correct after the record is copied and serialized again.

**Confidence:** Very high for the API contract. The clarification was authored and merged directly by John Thacker in `uat.h`.
