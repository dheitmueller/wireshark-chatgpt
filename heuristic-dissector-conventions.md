# Heuristic dissector conventions

## A heuristic must have protocol-identifying evidence

Do not register a dissector as heuristic when its test is effectively only a likely port and packet size and the protocol has no reliable on-wire discriminator. Such a test risks claiming unrelated traffic while giving users little control over the ambiguity.

Merged MR !20903, authored by Michael Mann and approved by Anders Broman, converted DCC away from a UDP heuristic because the old heuristic checked only port/size and there was no real identifying data in the packet. The replacement used a configurable UDP port preference even though the port is not IANA-registered. The same change adopted `udp_dissect_pdus()` so multiple DCC messages in one UDP datagram follow the standard PDU-looping machinery.

Practical guidance:

- Use heuristic registration only when packet content supplies a sufficiently discriminating signature or structural test.
- When identification fundamentally depends on deployment configuration such as a non-standard port, prefer a configurable port binding / Decode As-capable dispatch path over a weak heuristic.
- Once selected, use the transport's standard PDU helper when the protocol permits multiple messages in one transport unit instead of open-coding message iteration.

## Heuristics that override explicit protocol metadata should be opt-in

When a protocol carries an explicit dispatch hint such as HTTP `Content-Type`, prefer that declared metadata by default. If real deployments frequently provide incorrect or overly generic metadata and a content heuristic can improve dissection, expose heuristic-first behavior as a user preference rather than silently changing the default precedence.

Merged MR !19235, authored by John Thacker, added optional HTTP MIME sniffing. The accepted implementation keeps `Content-Type` dispatch first by default, provides a preference to try body heuristics first, and documents why the heuristic-first mode is disabled by default in light of RFC 9110 guidance. It also preserves and restores `pinfo->can_desegment` around heuristic probing so changing dispatch order does not leak temporary packet-state changes into later dissection.

Practical guidance:

- Treat explicit on-wire dispatch metadata as authoritative by default unless the protocol specification or established Wireshark behavior says otherwise.
- If heuristic inspection is useful specifically to override unreliable metadata, make that precedence change explicit and user-configurable.
- Choose the default with protocol interoperability and specification guidance in mind, not merely whichever path produces the richest dissection on a sample capture.
- When probing a heuristic requires temporarily changing `packet_info` or other shared dissection state, save and restore that state around the probe.
