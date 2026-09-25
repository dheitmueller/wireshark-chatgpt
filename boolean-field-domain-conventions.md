# Boolean Field Domain Conventions

This file records durable conventions for deciding when a protocol field is semantically Boolean rather than merely encoded with values 0 and 1. Current upstream source remains authoritative.

## An unmasked FT_BOOLEAN represents zero versus nonzero

For an unmasked `FT_BOOLEAN`, Wireshark's semantic domain is false for zero and true for any nonzero value. It is not an integer enumeration restricted to exactly 0 and 1. A protocol field whose currently assigned values happen to be 0 and 1 but whose other numeric values are reserved, invalid, or may later receive meanings should remain an integer field with a value table.

Merged master MR !9336, authored by Martin Mathieson, explicitly reverts several conversions to common true/false strings because when there is no bitmask any nonzero value is treated as true. The accepted changes restore `FT_UINT8`, `FT_UINT16`, or `FT_UINT32` registrations with `value_string` tables.

**Implementation rule:** choose `FT_BOOLEAN` because the protocol defines a truth predicate, not merely because the currently documented numeric values are 0 and 1. If the protocol defines a categorical numeric domain, use an integer field and map the assigned values explicitly.

**Review rule:** for an unmasked Boolean conversion, ask what an on-wire value of 2 would mean. If Wireshark would display it as true but the protocol would call it reserved, invalid, or a future enum value, the field should not be `FT_BOOLEAN`.

Later merged !9597 supplies the complementary positive case: when a protocol explicitly defines any nonzero value as true, `FT_BOOLEAN` is the correct representation.

**Confidence:** Very high. Direct merged correction authored by Martin Mathieson, independently complemented by a later merged Boolean-domain fix.
