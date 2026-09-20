# Wireshark UAT Schema-Evolution Conventions

This file records durable conventions for evolving User Accessible Table (UAT) schemas while preserving existing user configuration. Current upstream source and UAT API documentation remain authoritative.

## Append new fields with defaults so older UAT rows remain readable

Persisted UAT rows outlive a single Wireshark build, so adding a field changes a serialized user-facing schema. When a new release appends fields that old rows do not contain, register defaults for those missing fields instead of assuming every row was written by the current version.

Merged master MR !15181, authored and merged by Martin Mathieson with direct guidance from John Thacker, extended the PDCP-NR UAT with new configuration fields. John explained the compatibility behavior in both directions: an older Wireshark reading a newer row with extra fields can ignore the unknown fields with a warning, while a newer Wireshark reading an older row needs `uat_set_default_values()` so missing appended fields receive defined values. Martin incorporated the defaulting path before merge.

**Implementation rule:** when appending fields to an existing UAT, define meaningful defaults for rows created by older versions and register them through the UAT default-value mechanism. Treat absent persisted fields as a version-skew condition, not as uninitialized memory or an implicit zero unless zero is the intentional semantic default.

**Compatibility rule:** prefer additive schema evolution where older readers can tolerate unknown trailing fields and newer readers can synthesize missing trailing fields. If a change cannot be made additive, explicitly evaluate profile/configuration migration rather than assuming UAT persistence is ephemeral.

**Review rule:** for every UAT schema change, test or reason through both directions of version skew: old row -> new Wireshark and new row -> old Wireshark. Pay particular attention to booleans/enums where a default silently changes dissection behavior.

**Confidence:** Very high. Merged master dissector change by Martin Mathieson with explicit John Thacker guidance on the intended UAT compatibility API and behavior.
