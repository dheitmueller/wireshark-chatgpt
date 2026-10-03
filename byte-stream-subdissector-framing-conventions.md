# Wireshark Byte-Stream Subdissector Framing Conventions

This file records durable guidance for protocols carried over byte-stream transports.

## QUIC streams do not preserve application PDU boundaries

A QUIC STREAM presents an ordered byte stream. QUIC STREAM frame boundaries are transport artifacts and are not guaranteed to survive transmission, retransmission, reordering, or delivery to the application. A protocol dissector running on a QUIC stream therefore must not treat one STREAM frame or one packet as one application PDU.

Merged master MR !123 adds SMB over QUIC by registering the existing NetBIOS Session Service dissector for the QUIC ALPN. During review, Guy Harris explicitly compared QUIC with TCP: both expose byte streams without application packet boundaries, which is why SMB over TCP uses an NBSS-like length-bearing framing layer. Peter Wu notes that QUIC subdissectors can request stream reassembly through the standard `pinfo->desegment_offset` and `pinfo->desegment_len` fields, and Guy observes that the existing NBSS/SMB dissectors already use that contract.

**Implementation rule:** let the protocol layer that knows the application framing determine PDU boundaries. Reuse existing stream-framing/desegmentation logic when the same application protocol moves from TCP/TLS to QUIC; do not invent a QUIC-frame-bound parser unless the application protocol specification actually defines that mapping.

**Review rule:** when a new transport binding is proposed, ask whether the carrier is message-oriented or byte-stream-oriented. If it is a byte stream, test split PDUs, multiple PDUs in one delivery, retransmission/reassembly, and existing `pinfo` desegmentation behavior.

**Confidence:** Extremely high. Direct Guy Harris architecture guidance, corroborated by Peter Wu's QUIC implementation details, on a merged master MR.
