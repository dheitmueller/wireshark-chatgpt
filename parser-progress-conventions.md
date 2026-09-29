# Wireshark Parser Progress Conventions

This file records durable rules for parsers whose input position is controlled by packet-derived lengths or bounded textual fields. Current upstream source remains authoritative.

## Retained output bounds and consumed input length are different contracts

A parser may deliberately retain only a bounded prefix of an input field while still being required to consume the field's complete wire representation. Truncating the stored value must not truncate input advancement.

Merged master MR !11061 fixes the 3GPP log timestamp parser for fractional seconds containing more than nine digits. The decoder retains only the precision it can represent, but it advances over all fractional digits before continuing. The previous implementation stopped advancing once its retained digit count reached the supported precision, leaving the parser at the same input and permitting an infinite loop.

**Progress rule:** when a syntactic field can be longer than the representation retained by Wireshark, track consumed input independently from retained output. Every recoverable iteration must either advance through the complete syntactic unit or terminate/propagate failure.

**Review/testing rule:** test values longer than the implementation's retained precision or display capacity. Verify both the value produced and the final packet offset; a correct-looking truncated value can still hide a no-progress bug.

**Confidence:** High. Merged master correctness fix with a concrete infinite-loop failure mode.

## Reject zero-length loop elements before they can become no-progress iterations

When a repeated packet structure derives each element's size from the packet, a decoded size of zero is not merely a malformed value: if that size controls the outer offset, it is also a loop-termination hazard.

Merged master MR !11054 fixes WiMAX ASN Control Plane parsing where malformed zero-byte lengths could leave the loop offset unchanged. The accepted change reports the malformed condition and breaks rather than attempting another iteration. The same change replaces a raw `tvb_get_ptr()` plus `memcpy()` path with `tvb_memcpy()`, keeping packet bounds attached to the copy operation.

**Progress rule:** before re-entering a variable-length element loop, prove that the chosen recovery path advances the input. If the protocol-derived element length is zero and zero cannot represent a valid consumable element, diagnose it and leave the loop.

**API rule:** for copying packet bytes, prefer TVBuff copy helpers such as `tvb_memcpy()` over extracting a raw pointer and invoking a generic memory routine; this preserves TVBuff bounds checking at the operation that consumes the bytes.

**Confidence:** Very high. Merged master malformed-input fix with an explicit no-progress condition and a TVBuff-native bounds-safety cleanup.

## Size parser cursors for the addressable input, not for the wire field that produced them

The width of an on-wire field does not define the correct C type for an in-memory parser offset. Once a wire value participates in additions, scanning, or repeated advancement through a tvbuff, the cursor must represent the parser's full addressable range and all intermediate arithmetic needed for termination.

Merged master MR !10756, authored and merged by Gerald Combs, fixes an XRA dissector infinite loop by widening parser cursors and derived DOCSIS offsets/lengths from narrow 16-bit storage to natural integer types. The motivating failure occurred when offset arithmetic overflowed even though the individual wire values themselves fit their protocol-defined widths. John Thacker tied the fix to the reported infinite-loop issue, and the commit message was updated accordingly.

**Progress rule:** choose cursor/index types from the maximum in-memory offset and arithmetic domain, not from the width of the packet field that initially supplied a value. A loop counter or next-offset expression must not be able to wrap back into the loop's valid range.

**Review rule:** whenever a packet-sized integer becomes an offset, inspect the type after every promotion/narrowing boundary and the type of the arithmetic expression itself. A bounds comparison after already-wrapped arithmetic is too late.

**Confidence:** High. Merged master infinite-loop fix with a concrete overflow-to-no-progress failure mode.



## Error returns must not masquerade as successful consumption

A parser helper's return value is often part of the caller's loop-control contract. An error path must not return a plausible positive byte count if the caller interprets positive values as successful consumption.

Merged master MR !5280 fixes a BT-DHT endless loop this way. When a compact-node string has an invalid length, the helper already adds expert information, but it formerly returned `tvb_reported_length_remaining(tvb, offset)`. The caller treated that positive value as a normal consumed length and could continue incorrectly. The accepted fix returns `0`, which is the helper's failure signal and causes the caller to report the error and terminate that parse path. Release-3.6 !5281 and release-3.4 !5282 carry the same correction.

**Progress rule:** define helper return values together with the caller's advancement/termination behavior. On a structural error, return the documented failure sentinel or propagate an explicit status; do not synthesize a positive “remaining” length merely to leave the helper.

**Review rule:** for every helper used inside a repeated parse, inspect both its malformed-input return and the caller's interpretation of that return. A locally reasonable error value can become a no-progress or excessive-work bug one stack frame up.

**Confidence:** Very high. Merged master endless-loop fix with two maintained-branch backports.


## Use modular comparison for wrapping sequence spaces, then bound incompatible traversals

Merged master MR !5190, authored by John Thacker, fixes an RTMPT infinite loop by replacing the raw comparison `tp->lastseq >= seq` with TCP's wrap-aware `GE_SEQ()`. Ordinary integer ordering is not valid across a wrapping TCP sequence-number space.

That correction was necessary but not sufficient for every RTMPT path. Later merged master MR !5225, also authored by John Thacker, stops a tree traversal when sequence wrap makes the lookup order itself ambiguous; release-3.6 !5226 and release-3.4 !5230 carry the same fix. Earlier !5213/!5214 provide additional wrap-aware comparison evidence.

**Progress rule:** use protocol-correct modular comparison for wrapping sequence spaces; never infer protocol ordering with ordinary relational operators on the encoded integer.

**Data-structure rule:** separately inspect the ordering semantics of any tree/map used to traverse that sequence space. A wrap-aware comparison does not make a linearly ordered container modular. If wraparound can cause ambiguous/repeating traversal, add a conservative termination condition even if it sacrifices some recovery in the rare edge case.

**Confidence:** Extremely high. Two merged master RTMPT infinite-loop fixes authored by John Thacker, with maintained-branch backports of the later traversal bound.
