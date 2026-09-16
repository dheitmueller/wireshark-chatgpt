# Wireshark Persisted Configuration Compatibility Conventions

This file records durable compatibility lessons for user-visible persisted configuration formats.

## Keep serialized field order stable; change visual order separately

When a UAT or other persisted configuration is serialized positionally, the stored field order is part of the compatibility contract. Adding a field in the middle of the serialized description can make configurations written by earlier Wireshark versions fail or be misinterpreted. Prefer appending new serialized fields and, when the UI should show the field elsewhere, change the visual presentation independently of the stored order.

Merged MR !18120, authored by John Thacker, fixed the I/O Graphs “Average over Time” addition by moving the new AOT field to the end of the UAT description/defaults while using Qt header section movement to retain the desired visual column location. The MR explicitly states that this preserves I/O Graph configurations from earlier versions. After merge, Stig Bjørlykke reported that no I/O graph appeared; John traced this to a parallel column-order enum and followed up with merged !18130. That follow-up is useful evidence that compatibility changes should audit every parallel representation of serialized/display ordering rather than changing only the obvious descriptor.

**Implementation rule:** treat positional persisted configuration schemas as compatibility-sensitive. Append fields where possible; decouple serialized order from visual order; and audit enums, defaults, import/export code, and other parallel index mappings whenever the stored ordering changes.

**Testing rule:** test loading a configuration produced by the previous supported version, not only creating and reading a configuration with the new build. Also exercise the normal UI path after migration so stale parallel order/index tables are detected.

**Confidence:** High. Both !18120 and its corrective follow-up !18130 were merged; the compatibility rationale and the post-merge regression are explicit in the MR discussion.