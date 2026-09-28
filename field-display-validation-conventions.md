# Field Display Validation Conventions

This file records durable field-registration validation rules from accepted Wireshark changes. Current upstream source remains authoritative.

## Display metadata must match the registered field type

Merged master MR !6181, authored by Guy Harris, reorganizes protocol-field validation so each field-type family explicitly documents the display-information forms it permits. The old switch structure contained fallthrough that could allow `BASE_PROTOCOL_INFO` for floating-point fields even though that display metadata has no semantic meaning for those types.

**Registration rule:** treat `hfinfo->display` as type-specific semantic metadata, not as a generic integer flag bag. Validate the chosen `BASE_*` or related display information against the registered `FT_*` type.

**Control-flow rule:** do not use switch fallthrough between unrelated field-type families merely to share rejection logic when doing so can accidentally broaden the accepted display domain.

**Diagnostic rule:** when registration is invalid, report which display-information category is permitted for that field type rather than emitting only a generic rejection.

**Confidence:** Extremely high. Merged master cleanup authored by Guy Harris, with the invalid combination and accidental fallthrough identified directly.
