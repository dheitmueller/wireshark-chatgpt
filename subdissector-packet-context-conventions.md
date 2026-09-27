# Subdissector Packet-Context Conventions

Merged MR !7035, authored by John Thacker, fixes TCP frames containing multiple upper-layer PDUs. An upper-layer dissector can legitimately rewrite packet addresses, port type, and ports, but TCP still needs its original endpoint tuple for reassembly and the next sibling PDU must not inherit the previous sibling's mutations.

**Rule:** when a lower-layer dissector invokes multiple sibling PDUs, save and restore the lower-layer packet context before each sibling and before lower-layer state operations.

Merged MR !7033 further shows that once dissection operates on a reconstructed TCP MSP, desegmentation offsets belong to that logical reassembled stream rather than the current physical segment.

**Confidence:** Very high. Both are merged TCP correctness fixes authored by John Thacker.
