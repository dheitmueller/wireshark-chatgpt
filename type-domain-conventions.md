# Wireshark Type-Domain Conventions

## Equal numeric values do not make independent semantic domains interchangeable

Merged master MR !7915, authored by Guy Harris, fixes MS Proxy state that stored packet-type `PT_*` values and cast them to `endpoint_type`. TCP and UDP happened to use the same numeric values in both enums, but that coincidence was not a valid contract. The accepted code stores `endpoint_type` directly and assigns `ENDPOINT_*` values.

**Rule:** when two enums or identifier spaces represent different concepts, keep the declared type and constants from the domain the API expects. Do not type-pun or cast between domains solely because current integer values happen to coincide.

Release-4.0 MR !7916 carries the same fix.

## Central predicates should define semantic type families

Merged master MR !7213 adds `FT_UINT_STRING` to `IS_FT_STRING()` and removes repeated one-off exceptions from fvalue string accessors.

**Type-system rule:** when a field type participates in a semantic family, encode that membership in the central family predicate and let generic APIs depend on it. Scattered `type == ...` exceptions make the type model inconsistent and easy to miss.

**Confidence:** High. Merged core ftypes change by João Valverde.

## Prefer semantically typed fvalue accessors over generic pointer extraction

Merged master MR !7201, authored by João Valverde, replaces generic pointer-style ftype getter entries and caller casts with typed accessors for strings, bytes, and other value domains.

**API rule:** when the field type determines a concrete semantic representation, expose that type in the accessor signature. Typed getters improve const-correctness, document the contract, and let the compiler catch mismatched value-domain use.

**Confidence:** Very high. Merged core ftypes API refactor by João Valverde.
