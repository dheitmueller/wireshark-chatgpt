# Field Value Semantics Conventions

This file records durable Wireshark conventions for representing protocol field values whose raw encodings have semantic special cases. Current upstream source remains authoritative.

## Keep protocol-specific sentinel meanings attached to the field

A raw value that sometimes represents a normal value and sometimes represents a protocol-specific sentinel such as “unknown”, “now”, or “cancel” should not be encoded as a global display-mode rule when different fields or protocols can assign different meanings to that same raw value. Keep the base field type semantically correct and attach the exceptional interpretation to the field through a value mapping or equivalent field-specific mechanism.

Merged !19694, authored by Guy Harris, extended `FT_ABSOLUTE_TIME` so a field can carry a `time_value_string` table for special raw time values. This supports cases such as Zigbee, where zero can mean “Now” and `0xffffffff` can mean cancellation rather than literal timestamps. In discussion, John Thacker asked whether the older `ABSOLUTE_TIME_NTP_UTC` display mode—where zero globally means unknown/null—should be deprecated in favor of this mechanism. Guy agreed, explicitly noting that different uses can require different strings for the special case. Merged !19704 then applied the mechanism to Zigbee UTCTime fields and added `TIME_VALS` to the API checker.

**Implementation rule:** distinguish the field's underlying data type from protocol-specific sentinel semantics. If identical raw values have context-dependent meanings, represent those meanings in field-local metadata/value tables rather than proliferating special global display modes or hard-coded formatter behavior.

**Confidence:** Extremely high. Merged framework change authored by Guy Harris, direct Guy Harris/John Thacker design discussion, and an immediate merged protocol conversion using the new mechanism.