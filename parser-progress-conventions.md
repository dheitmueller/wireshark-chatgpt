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

## Error-skipping parsers must preserve forward progress

Merged master MR !9752 (John Thacker) removes a CBOR chunk-read error-path rollback to `chunk->start` that defeated `wscbor_skip_if_errors()`. A failed parse could repeatedly revisit the same malformed container element, causing nontermination or memory exhaustion.

**Rule:** An error-skipping parser must either consume invalid input or terminate its caller. Do not silently restore a cursor to the same position when the caller may continue. Test malformed/excessive-length lists for monotonic offset progress. **Confidence:** Very high; accepted master fix.

## Advance a variable-length parser only if the child decoder has not consumed an offset

Pascal Quantin's merged master fix !9169, with stable backports !9177/!9178, repairs `dissect_per_restricted_character_string_sorted()`. In the empty/restricted-character-string case, the caller previously unconditionally incremented `offset`, even when `dissect_per_length_determinant()` had already advanced it. The accepted code records `old_offset` before the length operation and increments one unit only if the offset remains unchanged.

**Parser contract:** a nested helper that returns an updated cursor transfers ownership of that consumed span to the caller. Do not add a default increment after a successful advance; if a zero-length element requires progress, make that conditional on the helper returning an unchanged cursor, preserving alignment semantics. Test both length-determinant forms, empty values, and the field immediately following the string. **Confidence:** Very high; merged master fix and two accepted stable backports.

## Account for the parsed extension's length, not just its tag

Merged master !9034 fixes BER unknown sequence extensions: the identifier decoder's returned offset is passed to `dissect_ber_length()`, and the latter's returned offset plus declared `len` determines the entire covered field range. Previously ignoring the returned length cursor made multiple unknown extensions appear to overlap or attach later bytes to the preceding one. **Rule:** in tag-length-value streams, keep the identifier, length header, and value extent as distinct offsets; advance by the actual helper return and bound the unknown-field tree item to that single element. **Confidence:** high, merged targeted correctness diff.

## Bound streaming decompression by both progress and resource budgets

Merged release fixes !9014/!9015 (João Valverde) replace an unconditional Kafka Zstd `do` loop with a condition on unconsumed input. Merged !9020/!9021 (Gerald Combs) additionally cap iteration counts for legacy Kafka decompression loops and avoid repeated uncontrolled allocations. **Rule:** loops over untrusted compressed frames need a source-consumption condition, explicit failure on non-progress and bounded work/memory; resource caps complement, rather than replace, correct input exhaustion and ownership. **Historical limitation:** these fixes are release-branch implementations; merged master !9060 later centralizes Zstd decode in TVBuff helpers, so prefer that architecture over copying the old Kafka loop. **Confidence:** high for the failure modes, with the later migration given more architectural weight.
