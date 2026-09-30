# Wireshark TCP Reassembly Perspective Conventions

Merged master MR !7949, authored by John Thacker, distinguishes sender-oriented TCP retransmission analysis from the reassembler's record of sequence bytes already used in an active multi-segment PDU. Those are different questions and can produce different answers.

The accepted change gives out-of-order reassembly its own old-data state and adds tests covering retransmissions before and after reassembly completion.

**Rule:** use sequence state whose definition matches the consumer. Do not reuse a nearby retransmission flag when that flag describes sender behavior rather than capture-side reassembly behavior.

**Testing rule:** cover out-of-order delivery, overlap/retransmission, completion, and missing capture segments.


## Validate a plausible captured boundary before length-driven stream reassembly

A capture can begin in the middle of an established TCP stream. In that situation the first captured bytes are not guaranteed to be a protocol header, so feeding them directly to a generic length-driven PDU engine can interpret continuation data as a bogus length and poison later reassembly.

Merged master MR 4312, authored by John Thacker, changes SMPP to validate the minimum fixed header, legal command length, known command ID, and known status before entering `tcp_dissect_pdus()`. If the current capture boundary does not plausibly begin an SMPP PDU, the ordinary dissector returns zero. The heuristic path performs additional checks and, after successful recognition, binds the conversation so later TCP segmentation can be handled normally.

**Framing rule:** for stream protocols where capture can start mid-connection, separate “does this captured offset plausibly begin a PDU?” from “what length does this PDU claim?” Do not invoke a length-based reassembly framework until enough independent framing invariants pass.

**Heuristic rule:** conversation binding is a consequence of successful protocol recognition; it is not a substitute for proving the initial boundary from arbitrary midstream bytes.

**Confidence:** Very high. Merged master stream-dissection correction authored by John Thacker with the failure mode and intended recovery described directly in the MR.
