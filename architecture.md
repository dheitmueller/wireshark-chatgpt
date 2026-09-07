# Wireshark Architecture Notes

This is a curated cache of architectural understanding relevant to future development. It is intentionally incomplete; add material when it becomes useful for actual work.

## Operating rule

When changing a subsystem, trace the real call path in the current source before designing the patch. Prefer existing registration, dispatch, reassembly, conversation, and dissector-table mechanisms over adding parallel infrastructure.

## Extension points and protocol coupling

- Prefer generic extension points in a generic protocol dissector over embedding knowledge of one downstream protocol. MR !26376 is moving CoAP away from a dedicated Thread/TMF mode toward a generic heuristic payload subdissector list. The MR is still open, so treat its exact implementation as provisional, but use the separation-of-concerns principle when evaluating new designs.
- Avoid pushing consumer-specific protocol identity or dependencies down into generic transport/security dissectors when the consumer can resolve what it needs through an existing API. In MR !26374, Anders Broman requested that RADIUS resolve its relevant protocol ID locally with `proto_get_id_by_short_name()` rather than changing both TLS and DTLS to expose RADIUS-specific plumbing.
- Registration choices carry semantic meaning. A fixed TCP/UDP port table entry says more than “this often runs here”; do not claim an unassigned/dynamic port. Use Decode As or a well-founded heuristic mechanism instead.

## Tree visibility versus filterability

A protocol-tree item need not be visibly rendered to remain useful for display filtering. MR !26393 demonstrates adding eCPRI fields and calling `proto_item_set_hidden()` when O-RAN FH owns the visible dissection. This is useful when a parent/encapsulation protocol has filter semantics that should remain available without duplicating visible presentation.

## Areas of particular interest

- `epan/` dissector APIs and protocol-tree construction.
- RTP payload dispatch and media-over-IP dissectors.
- MPEG PES payload dispatch.
- Ancillary-data parsing and code reuse between ST 2110-40 / ST 2038 / related SMPTE transports.
- Test and fuzz target integration.

## Provenance

Architectural claims should ultimately cite current upstream files, commits, or review discussions in prose so stale assumptions are easier to detect and revalidate.
