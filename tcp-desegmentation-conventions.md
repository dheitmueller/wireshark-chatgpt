# Wireshark TCP Desegmentation Conventions

This file records durable conventions for TCP-based dissectors that use Wireshark's stream desegmentation helpers. Treat current upstream source and API contracts as authoritative.

## Heuristic recognition and `tcp_dissect_pdus()`

- Do not continue heuristic accept/reject decisions after `tcp_dissect_pdus()` has requested more TCP data. Once a dissector asks the TCP layer for desegmentation, later rejecting the packet violates the desegmentation contract and can produce incorrect behavior on short initial segments.
- For a protocol that is registered on an assigned TCP port but still performs a recognition check, perform the recognition first. Once the packet passes that check, bind the conversation to a non-heuristic stream dissector and let that dissector use `tcp_dissect_pdus()` for subsequent segments/PDUs.
- Do not try to reconstruct whether `tcp_dissect_pdus()` succeeded by separately recomputing PDU lengths and then deciding to reject the packet. Stream ownership and desegmentation state must be consistent across segment boundaries.
- Merged MR !10154, authored and merged by John Thacker, is the direct exemplar. The XOT dissector could reject traffic after `tcp_dissect_pdus()` had requested another segment, notably when the first TCP payload was only one byte. The accepted fix separates the initial recognition path from the conversation-bound TCP dissector, sets the conversation dissector after recognition, and leaves `tcp_dissect_pdus()` responsible for stream reassembly thereafter.

## Relationship to general heuristic guidance

These rules complement `dissector-conventions.md`: heuristic probes must be safe on arbitrary traffic and may decline nonmatches, but once a stateful TCP dissector has committed to desegmentation it must not subsequently behave as though the same packet were merely an unclaimed heuristic candidate.
