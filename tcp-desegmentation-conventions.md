# Wireshark TCP Desegmentation Conventions

This file records durable conventions for TCP-based dissectors that use Wireshark's stream desegmentation helpers. Treat current upstream source and API contracts as authoritative.

## Heuristic recognition and `tcp_dissect_pdus()`

- Do not continue heuristic accept/reject decisions after `tcp_dissect_pdus()` has requested more TCP data. Once a dissector asks the TCP layer for desegmentation, later rejecting the packet violates the desegmentation contract and can produce incorrect behavior on short initial segments.
- For a protocol that is registered on an assigned TCP port but still performs a recognition check, perform the recognition first. Once the packet passes that check, bind the conversation to a non-heuristic stream dissector and let that dissector use `tcp_dissect_pdus()` for subsequent segments/PDUs.
- Do not try to reconstruct whether `tcp_dissect_pdus()` succeeded by separately recomputing PDU lengths and then deciding to reject the packet. Stream ownership and desegmentation state must be consistent across segment boundaries.
- Merged MR !10154, authored and merged by John Thacker, is the direct exemplar. The XOT dissector could reject traffic after `tcp_dissect_pdus()` had requested another segment, notably when the first TCP payload was only one byte. The accepted fix separates the initial recognition path from the conversation-bound TCP dissector, sets the conversation dissector after recognition, and leaves `tcp_dissect_pdus()` responsible for stream reassembly thereafter.

## Keep `tcp_dissect_pdus()` length callbacks bounded and non-throwing

The callback used to determine a TCP PDU's length is part of the stream-framing mechanism and must be safe on both truncated and malformed input. If a protocol uses a variable-length integer in its framing header, enforce the protocol's maximum encoding length before shifts or accumulations can become undefined. While a still-valid header is merely truncated, return/request more data; once the maximum legal prefix has been consumed without termination, treat the header as malformed and choose a bounded recovery path rather than letting an exception escape through the `tcp_dissect_pdus()` callback.

Merged MR !16517, authored and merged by John Thacker, replaces NMF's hand-rolled variable-length length parser with Wireshark's varint API and caps the length prefix at the protocol's five-byte maximum. The previous code could continue shifting beyond a valid width, and its framing callback could throw on malformed/truncated input. The accepted implementation distinguishes a legal-but-incomplete prefix from an overlong malformed one and supplies a safe PDU length for the latter so dissection can continue without violating the stream helper's callback contract.

**Implementation rule:** framing callbacks must not use unbounded arithmetic or depend on exceptions to signal malformed framing. Bound variable-width prefixes by the protocol specification, distinguish truncation from invalid overlength, and return a deterministic length/reassembly decision to the stream helper.

**Confidence:** Very high. Merged master stream-framing fix authored and merged by John Thacker.

## Relationship to general heuristic guidance

These rules complement `dissector-conventions.md`: heuristic probes must be safe on arbitrary traffic and may decline nonmatches, but once a stateful TCP dissector has committed to desegmentation it must not subsequently behave as though the same packet were merely an unclaimed heuristic candidate.
