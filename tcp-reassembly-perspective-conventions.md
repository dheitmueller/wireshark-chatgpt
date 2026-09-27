# Wireshark TCP Reassembly Perspective Conventions

Merged master MR !7949, authored by John Thacker, distinguishes sender-oriented TCP retransmission analysis from the reassembler's record of sequence bytes already used in an active multi-segment PDU. Those are different questions and can produce different answers.

The accepted change gives out-of-order reassembly its own old-data state and adds tests covering retransmissions before and after reassembly completion.

**Rule:** use sequence state whose definition matches the consumer. Do not reuse a nearby retransmission flag when that flag describes sender behavior rather than capture-side reassembly behavior.

**Testing rule:** cover out-of-order delivery, overlap/retransmission, completion, and missing capture segments.
