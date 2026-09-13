# Wireshark API Domain Conventions

This file records durable API type-domain conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Encode inherently non-negative quantities with non-negative types

Lengths, byte counts, offsets, and other quantities that cannot legitimately be negative should normally use an unsigned type with enough range for the underlying protocol or API. A signed type that is wider than current inputs can still encode impossible states, invite accidental negative arithmetic, and obscure the real range contract.

A cluster of merged MRs provides unusually strong evidence for this rule. In !25653, John Thacker changed multiple proto APIs that return item lengths from signed to unsigned because their results are inherently non-negative. In !25655 and !25651, Guy Harris similarly changed wiretap byte-count state and arithmetic to unsigned 64-bit values. In !25625, John Thacker widened RELOAD parser offsets after packet-derived 32-bit quantities had been stored in 16-bit variables; release backports !25627 and !25628 preserved the fix.

Merged master MR !21574 supplies additional parser-specific evidence. An ASN.1 PER choice index is decoded from the packet as an unsigned value; converting it to signed `int` before further index arithmetic created signed-overflow undefined behavior under OSS-Fuzz. John Thacker's accepted fix keeps the value in `uint32_t`, preserving the wire field's actual domain through the arithmetic.

**Implementation rule:** choose integer types from the semantic domain and maximum representable input, not from historical convenience. For packet/file offsets and lengths, verify that the type can represent the full wire or buffer range and avoid signedness that implies impossible negative values. Keep packet-derived enumerators, indices, and counts in their non-negative domain unless a later API genuinely requires a signed representation.

**Confidence:** Extremely high. Multiple merged master changes from Guy Harris and John Thacker, with accepted stable-branch propagation and independent OSS-Fuzz evidence.

## Normalize sentinel values at the API boundary, then keep internal APIs in the real domain

A public or compatibility-facing API may need a special signed sentinel such as `-1`, but that does not require every internal helper to carry the sentinel forever. Convert the sentinel once at the layer that owns its meaning, then pass an ordinary value in the narrower semantic domain to lower-level helpers.

Merged MR !25622 is a strong exemplar. John Thacker changed internal proto-tree text helpers to accept unsigned lengths, while `proto_tree_add_subtree_format()`—the layer whose contract still permits `-1`—converts that sentinel into `tvb_captured_length_remaining()` before calling the internal unsigned helper. This removes a special case from lower layers without breaking the boundary contract.

Merged MR !21738 adds a complementary boundary-validation detail. WSLua `TreeItem_set_len` must preserve the documented `-1` sentinel, but after Jaap Keuter questioned an implementation that accepted every negative value, the merged code was tightened to reject values below `-1`. A distinguished sentinel does not imply that the entire otherwise-invalid numeric region shares its meaning.

Merged master MR !21558, authored and merged by John Thacker, adds a further control-flow consequence. SSH uses `mac_length == -1` for "unknown". Testing that signed value with plain C truthiness incorrectly treats `-1` as a usable MAC length; the accepted fix tests `mac_length > 0` before consuming bytes and normalizes non-positive values before using them as an actual size. Stable backport !21559 preserves the correction.

**Implementation rule:** keep exceptional encodings and compatibility sentinels at the smallest boundary that needs them. Normalize them before entering internal code so lower layers can express and enforce their true invariants in the type system. When validating the compatibility-facing boundary itself, accept exactly the documented sentinel values; do not generalize a special value such as `-1` into “any negative value” unless the API contract explicitly assigns meaning to that whole range. When a signed field mixes a negative sentinel with positive sizes or counts, test the semantic domain explicitly (`> 0`, `== sentinel`, etc.); do not use boolean truthiness as a proxy for validity.

**Confidence:** Very high. Merged master API cleanup by John Thacker, merged WSLua boundary validation revised in response to direct Jaap Keuter review, and a merged John Thacker SSH correctness fix with stable backport.

## Consume aliased inputs before mutating an output parameter

When an API permits an output object to alias one of its input objects, its implementation must preserve the original input values until every calculation that needs them has completed. Writing even one output member too early can silently corrupt later calculations that still read through the aliased input pointer.

Merged MR !25873, authored and merged by John Thacker, fixes `nstime_delta()` when the result pointer is the same object as its first input. The accepted implementation rearranges the subtraction and normalization so that all required values from the original operands are consumed before the result mutation can destroy them.

**Implementation rule:** for in-place-capable helpers, explicitly reason about every supported alias combination. Compute from temporaries or order reads before writes so output mutation cannot change an input value that remains semantically live. Add an aliasing regression test when the API contract permits in-place operation.

**Confidence:** Extremely high. Merged master correctness fix authored and merged by John Thacker for a concrete output/input-aliasing failure.

## Normalize representation units at the boundary where their semantic meaning changes

A count should have one unambiguous unit once it enters a semantic layer. Carrying a textual representation count as though it were a packet byte count invites inconsistent arithmetic, especially when comments and encoded packet records use different relationships between source characters and output bytes.

Merged MR !25877, authored and merged by Guy Harris, changes the DCT2000 reader to convert the input character count into a record byte count at the point where packet-record sizing is established. Comment records remain one character per byte, while non-comment packet data uses two ASCII hexadecimal characters per byte. The change then carries the byte-oriented value through the record-length logic instead of repeatedly interpreting a mixed-unit variable.

**Implementation rule:** name and normalize length/count variables according to the unit consumed by the layer that owns them. When parsing textual encodings, convert character/nibble/word counts to byte counts once at a clear boundary and keep subsequent allocation, bounds, and record-length arithmetic in that normalized unit unless a later conversion is explicit.

**Confidence:** Extremely high. Merged master wiretap cleanup authored and merged by Guy Harris, directly clarifying the unit contract of packet-record sizing.
