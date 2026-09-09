# Wireshark API Domain Conventions

This file records durable API type-domain conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Encode inherently non-negative quantities with non-negative types

Lengths, byte counts, offsets, and other quantities that cannot legitimately be negative should normally use an unsigned type with enough range for the underlying protocol or API. A signed type that is wider than current inputs can still encode impossible states, invite accidental negative arithmetic, and obscure the real range contract.

A cluster of merged MRs provides unusually strong evidence for this rule. In !25653, John Thacker changed multiple proto APIs that return item lengths from signed to unsigned because their results are inherently non-negative. In !25655 and !25651, Guy Harris similarly changed wiretap byte-count state and arithmetic to unsigned 64-bit values. In !25625, John Thacker widened RELOAD parser offsets after packet-derived 32-bit quantities had been stored in 16-bit variables; release backports !25627 and !25628 preserved the fix.

**Implementation rule:** choose integer types from the semantic domain and maximum representable input, not from historical convenience. For packet/file offsets and lengths, verify that the type can represent the full wire or buffer range and avoid signedness that implies impossible negative values.

**Confidence:** Extremely high. Multiple merged master changes from Guy Harris and John Thacker, with accepted stable-branch propagation.

## Normalize sentinel values at the API boundary, then keep internal APIs in the real domain

A public or compatibility-facing API may need a special signed sentinel such as `-1`, but that does not require every internal helper to carry the sentinel forever. Convert the sentinel once at the layer that owns its meaning, then pass an ordinary value in the narrower semantic domain to lower-level helpers.

Merged MR !25622 is a strong exemplar. John Thacker changed internal proto-tree text helpers to accept unsigned lengths, while `proto_tree_add_subtree_format()`—the layer whose contract still permits `-1`—converts that sentinel into `tvb_captured_length_remaining()` before calling the internal unsigned helper. This removes a special case from lower layers without breaking the boundary contract.

**Implementation rule:** keep exceptional encodings and compatibility sentinels at the smallest boundary that needs them. Normalize them before entering internal code so lower layers can express and enforce their true invariants in the type system.

**Confidence:** Very high. Merged master API cleanup by John Thacker, approved and merged by Anders Broman.