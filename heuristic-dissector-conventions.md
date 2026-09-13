# Heuristic dissector conventions

## A heuristic must have protocol-identifying evidence

Do not register a dissector as heuristic when its test is effectively only a likely port and packet size and the protocol has no reliable on-wire discriminator. Such a test risks claiming unrelated traffic while giving users little control over the ambiguity.

Merged MR !20903, authored by Michael Mann and approved by Anders Broman, converted DCC away from a UDP heuristic because the old heuristic checked only port/size and there was no real identifying data in the packet. The replacement used a configurable UDP port preference even though the port is not IANA-registered. The same change adopted `udp_dissect_pdus()` so multiple DCC messages in one UDP datagram follow the standard PDU-looping machinery.

Practical guidance:

- Use heuristic registration only when packet content supplies a sufficiently discriminating signature or structural test.
- When identification fundamentally depends on deployment configuration such as a non-standard port, prefer a configurable port binding / Decode As-capable dispatch path over a weak heuristic.
- Once selected, use the transport's standard PDU helper when the protocol permits multiple messages in one transport unit instead of open-coding message iteration.
