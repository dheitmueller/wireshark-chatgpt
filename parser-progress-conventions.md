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

