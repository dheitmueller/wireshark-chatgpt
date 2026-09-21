# Wireshark Persisted Configuration Compatibility Conventions

This file records durable compatibility lessons for user-visible persisted configuration formats.

## Keep serialized field order stable; change visual order separately

When a UAT or other persisted configuration is serialized positionally, the stored field order is part of the compatibility contract. Adding a field in the middle of the serialized description can make configurations written by earlier Wireshark versions fail or be misinterpreted. Prefer appending new serialized fields and, when the UI should show the field elsewhere, change the visual presentation independently of the stored order.

Merged MR !18120, authored by John Thacker, fixed the I/O Graphs “Average over Time” addition by moving the new AOT field to the end of the UAT description/defaults while using Qt header section movement to retain the desired visual column location. The MR explicitly states that this preserves I/O Graph configurations from earlier versions. After merge, Stig Bjørlykke reported that no I/O graph appeared; John traced this to a parallel column-order enum and followed up with merged !18130. That follow-up is useful evidence that compatibility changes should audit every parallel representation of serialized/display ordering rather than changing only the obvious descriptor.

**Implementation rule:** treat positional persisted configuration schemas as compatibility-sensitive. Append fields where possible; decouple serialized order from visual order; and audit enums, defaults, import/export code, and other parallel index mappings whenever the stored ordering changes.

**Testing rule:** test loading a configuration produced by the previous supported version, not only creating and reading a configuration with the new build. Also exercise the normal UI path after migration so stale parallel order/index tables are detected.

**Confidence:** High. Both !18120 and its corrective follow-up !18130 were merged; the compatibility rationale and the post-merge regression are explicit in the MR discussion.

## Accept harmless legacy input variants without changing the canonical serialized form

A persisted configuration reader can often become more tolerant without changing the format that Wireshark writes. That distinction matters when newer Wireshark versions must continue producing files that older supported versions can read.

Merged master MR !15097, authored by John Thacker and approved/merged by Gerald Combs, changes UAT boolean parsing to accept `TRUE` and `FALSE` case-insensitively while deliberately continuing to write the historical uppercase spellings. The MR explicitly cites backward compatibility with pre-4.4 Wireshark as the reason not to change the emitted representation. The Qt UAT model likewise keeps recognizing the older `Enabled` spelling used by legacy I/O Graph configurations while accepting the broadened boolean input.

**Implementation rule:** when broadening a persisted-format parser for harmless spelling, case, or legacy-value variants, keep the writer's canonical representation stable unless there is a deliberate format-version transition. Reader tolerance and writer compatibility are separate policy decisions; accepting more input does not require emitting a new form.

**Testing rule:** exercise both directions of compatibility: load representative older/variant input with the new parser, and verify that newly written configuration still uses the canonical representation expected by older supported readers.

**Confidence:** Very high. Merged master compatibility change authored by John Thacker and approved/merged by Gerald Combs, with the writer-side backward-compatibility requirement stated explicitly.