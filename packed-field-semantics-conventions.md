# Packed Field and Presentation Conventions

## Mask packed flags to the semantic subfield before enum interpretation

Merged master MR !2841, authored by Guy Harris, changes ETW direction handling to write named `PACK_FLAGS_DIRECTION_INBOUND` / `PACK_FLAGS_DIRECTION_OUTBOUND` values and to switch on `pack_flags & PACK_FLAGS_DIRECTION_MASK` rather than the entire flags word.

**Rule:** a packed flags word is not itself an enum. When one group of bits has enum semantics, isolate that group with its defined mask before comparing it with enum constants. Unrelated flags must not change the decoded value.

**Confidence:** extremely high. Merged master correctness cleanup authored by Guy Harris.

## Preserve the wire type and attach printable presentation through field metadata

Merged master MR !2822 fixes PFCP UE IP Address Pool Identity parsing. Anders Broman explicitly rejected converting the protocol's OCTET STRING into a string solely for display. The accepted field remains `FT_BYTES` and uses `BASE_SHOW_ASCII_PRINTABLE`.

**Rule:** register what the protocol carries on the wire. If byte-oriented data often has a useful printable rendering, use field display metadata rather than changing the semantic field type to `FT_STRING`.

**Confidence:** very high. Direct Anders Broman review incorporated into the merged implementation.

## Prefer declarative value tables for fixed numeric mappings

During merged master MR !2834, Martin Mathieson requested replacing custom formatting callbacks for fixed Keysight/Ixia NetFlow mappings with ordinary `value_string` tables referenced through `VALS()`. The accepted implementation follows that approach.

**Rule:** when a numeric value maps directly to a finite set of labels, express the mapping in field metadata. Reserve custom formatting callbacks for presentation that actually requires computation or context.

**Confidence:** high. Direct maintainer review reflected in merged code.
