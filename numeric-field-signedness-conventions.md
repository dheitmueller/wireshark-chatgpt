# Wireshark Numeric Field Signedness Conventions

This file records durable conventions for parsers that decode numeric wire values into registered protocol fields. Current upstream APIs and field definitions remain authoritative.

## Preserve the registered field's signedness when a parser serves both signed and unsigned fields

A wire encoding can use the same byte layout for values that are semantically signed or unsigned. If one parser helper accepts an arbitrary `hf_index`, it must not silently force every value through an unsigned representation merely because the bytes can be accumulated into an unsigned C type. The registered field type is part of the parser contract.

Merged master MR !12812, authored by John Thacker and merged by Anders Broman, fixes OER unconstrained INTEGER handling. OER encodes these values as variable-size signed numbers, while the helper can be used with either signed or unsigned registered fields depending on constraints. The accepted implementation consults the registered field type, sign-extends short negative encodings before accumulating bytes, and then uses `proto_tree_add_int()` or `proto_tree_add_uint()` to match the field type. An unexpected registered type is treated as an internal invariant failure rather than being coerced silently.

**Implementation rule:** when a shared numeric parser can populate multiple registered integer field types, derive signedness from the field/API contract, perform any required sign extension while decoding the wire representation, and add the result with the matching signed or unsigned protocol-tree API. Do not rely on same-width C storage to preserve signed semantics automatically.

**Review rule:** inspect generic numeric helpers for hidden assumptions about `FT_INT*` versus `FT_UINT*`, especially when values are variable-width, sign-extended, or returned through an output parameter. A field-width match alone does not establish equivalent interpretation.

**Confidence:** Very high. Merged master parser fix authored by John Thacker, with the field-type distinction encoded explicitly in the accepted implementation.
