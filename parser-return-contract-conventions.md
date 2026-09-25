# Wireshark Parser Return-Contract Conventions

This file records durable conventions for parser helper return values extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Error paths must preserve cursor semantics promised by the helper

A parser helper that returns a cursor, end offset, or consumed extent has a contract that applies on malformed-input paths as well as on success. Returning a generic failure sentinel such as zero is unsafe when callers interpret the result as a new position: the cursor can move backward, item lengths can become negative, and an enclosing loop can lose monotonic progress.

Merged master MR !14707, authored and merged by John Thacker, fixes `dissect_parameter_sequence()` in the RTPS dissector. The helper is defined to return the end offset of the parameter sequence. Two malformed-input exits returned zero even though the caller expected an end position. That could move the caller backward and produced repeated `proto_item_set_len()` assertions with negative lengths. The accepted fix returns `offset + size` on those failure paths, preserving the helper's positional contract while still reporting expert information for the malformed sequence.

**Implementation rule:** when a parser API returns an offset/extent, define what the return value means on every exit. If malformed input terminates parsing, return a position consistent with the bounded region already handed to the helper rather than an unrelated sentinel that violates monotonicity. Reserve sentinel/error returns for APIs whose callers explicitly distinguish them from valid positions.

**Review rule:** audit every early return in cursor-producing helpers. Ask whether a caller can assign the result directly to its offset, use it to size a tree item, or use it as the next loop position. Error handling must not break those downstream invariants.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker with the concrete negative-length failure mode documented in the MR. This complements, rather than replaces, the notebook's existing loop-level monotonic-progress guidance in `parser-edge-cases.md`.

## Distinguish an absolute next offset from a consumed length

A parser helper returning the next cursor position is not interchangeable with one returning a number of bytes consumed. Both are integers and can look plausible in ordinary packets, but treating an absolute offset as a length—or converting a failure path to "bytes remaining" when callers expect a next offset—can move the caller backward or leave it without progress.

Merged master MR !10588, authored and merged by John Thacker, fixes GDSDB's `add_uint_string()`. The helper's contract is to return the next offset. On an invalid length it returned `tvb_reported_length_remaining(tvb, offset)`, which is a length relative to the current cursor rather than an absolute next position. The accepted fix returns `tvb_reported_length(tvb)`, placing the caller at the end of the reported tvbuff and guaranteeing progress. Release-4.0 and release-3.6 backports !10591 and !10592 preserve the same correction.

**Implementation rule:** document and preserve the coordinate system of parser return values: absolute next offset, consumed byte count, remaining length, or bounded end position. Convert between those forms explicitly at the boundary where the semantic change occurs; do not substitute one merely because its numeric type matches.

**Review rule:** for helpers whose result is assigned back to an offset, inspect every success and failure return and ask whether it is in the caller's coordinate system. Malformed-input recovery must remain monotonic even when the normal decoded value cannot be produced.

**Confidence:** Very high. Merged master parser-progress fix authored by John Thacker and carried to two stable branches.
