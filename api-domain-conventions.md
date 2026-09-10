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

## Preserve dependency-defined scalar types at API call boundaries

Internal type modernization does not justify changing the scalar type required by an external C API. Types that appear semantically equivalent can differ in representation, width, calling convention, or how a dependency reads their storage; use the dependency's declared type where values cross that boundary.

Merged MR !24981 converted many internal GLib `gboolean` uses to C99 `bool`, but review by Pascal Quantin and Jaap Keuter identified GLib calls whose API contract specifically requires `gboolean`. Jaap noted that `gboolean` and `bool` are not interchangeable and that inappropriate substitution can produce wrong behavior on big-endian systems. Those API-boundary cases were restored before the MR merged.

**Implementation rule:** use modern project-native types for internal state where appropriate, but preserve the exact dependency-defined scalar type for arguments, outputs, callbacks, or storage passed through an external API. Do not perform mechanical `gboolean`/`bool`-style substitution across library boundaries merely because both represent truth values.

**Confidence:** Very high. Merged master cleanup corrected during maintainer review specifically to preserve the GLib API contract.

## Consume aliased inputs before mutating an output parameter

When an API permits an output object to alias one of its input objects, its implementation must preserve the original input values until every calculation that needs them has completed. Writing even one output member too early can silently corrupt later calculations that still read through the aliased input pointer.

Merged MR !25873, authored and merged by John Thacker, fixes `nstime_delta()` when the result pointer is the same object as its first input. The accepted implementation rearranges the subtraction and normalization so that all required values from the original operands are consumed before the result mutation can destroy them.

**Implementation rule:** for in-place-capable helpers, explicitly reason about every supported alias combination. Compute from temporaries or order reads before writes so output mutation cannot change an input value that remains semantically live. Add an aliasing regression test when the API contract permits in-place operation.

**Confidence:** Extremely high. Merged master correctness fix authored and merged by John Thacker for a concrete output/input-aliasing failure.
