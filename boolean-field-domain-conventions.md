# Boolean Field Domain Conventions

This file records durable conventions for deciding when a protocol field is semantically Boolean rather than merely encoded with values 0 and 1. Current upstream source remains authoritative.

## An unmasked FT_BOOLEAN represents zero versus nonzero

For an unmasked `FT_BOOLEAN`, Wireshark's semantic domain is false for zero and true for any nonzero value. It is not an integer enumeration restricted to exactly 0 and 1. A protocol field whose currently assigned values happen to be 0 and 1 but whose other numeric values are reserved, invalid, or may later receive meanings should remain an integer field with a value table.

Merged master MR !9336, authored by Martin Mathieson, explicitly reverts several conversions to common true/false strings because when there is no bitmask any nonzero value is treated as true. The accepted changes restore `FT_UINT8`, `FT_UINT16`, or `FT_UINT32` registrations with `value_string` tables.

**Implementation rule:** choose `FT_BOOLEAN` because the protocol defines a truth predicate, not merely because the currently documented numeric values are 0 and 1. If the protocol defines a categorical numeric domain, use an integer field and map the assigned values explicitly.

**Review rule:** for an unmasked Boolean conversion, ask what an on-wire value of 2 would mean. If Wireshark would display it as true but the protocol would call it reserved, invalid, or a future enum value, the field should not be `FT_BOOLEAN`.

Later merged !9597 supplies the complementary positive case: when a protocol explicitly defines any nonzero value as true, `FT_BOOLEAN` is the correct representation.

**Confidence:** Very high. Direct merged correction authored by Martin Mathieson, independently complemented by a later merged Boolean-domain fix.

## Preserve the protocol's declared scalar domain even when the named states look Boolean

A pair of human-readable values such as "Enabled/Disabled" or "On/Off" does not by itself make an on-wire field Boolean. Preserve the field type and domain defined by the protocol specification, especially when the protocol defines a full-width scalar rather than a one-bit predicate.

Merged MR !9281 converted a number of 0/1 integer fields to `FT_BOOLEAN` with shared `true_false_string` tables. During review, Pascal Quantin called out the MBIM cases specifically: the MBIM specification defines those fields as 32-bit integer values, not as single-bit Boolean fields, so they should retain their integer representation. Merged follow-up !9298, authored and merged by Martin Mathieson, restores those MBIM fields to `FT_UINT32` plus explicit `value_string` tables.

**Implementation rule:** use the protocol's semantic data model, not merely its current display vocabulary, to choose `FT_BOOLEAN` versus `FT_UINT*`. A scalar enum with presently assigned values 0 and 1 remains an integer enum when other numeric values are reserved, invalid, or available for future assignment.

**Review rule:** before replacing a two-entry `value_string` with a common TFS, verify both the specification's declared field type and the meaning of values outside 0 and 1.

**Confidence:** Very high. Direct Pascal Quantin review on a merged cleanup, followed immediately by a merged corrective MR from Martin Mathieson.
