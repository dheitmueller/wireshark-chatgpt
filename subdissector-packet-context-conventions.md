# Subdissector Packet-Context Conventions

Merged MR !7035, authored by John Thacker, fixes TCP frames containing multiple upper-layer PDUs. An upper-layer dissector can legitimately rewrite packet addresses, port type, and ports, but TCP still needs its original endpoint tuple for reassembly and the next sibling PDU must not inherit the previous sibling's mutations.

**Rule:** when a lower-layer dissector invokes multiple sibling PDUs, save and restore the lower-layer packet context before each sibling and before lower-layer state operations.

Merged MR !7033 further shows that once dissection operates on a reconstructed TCP MSP, desegmentation offsets belong to that logical reassembled stream rather than the current physical segment.

**Confidence:** Very high. Both are merged TCP correctness fixes authored by John Thacker.


## Do not forward opaque dissector data across incompatible caller/callee contracts

The `void *data` dissector argument is type-erased in C, but it is not semantically untyped. Its concrete meaning is determined by the dispatch path and the receiving dissector's contract. Passing an unrelated parent's context pointer to a child merely because both signatures use `void *` can reinterpret one structure as another and produce invalid pointers or state.

Merged master MR !5944, authored by Pascal Quantin, fixes 5G SMS dissection through HTTP/2. Stig Bjørlykke identified that the HTTP/2 multipart path supplies an `http_message_info_t`, while the DTAP dissector interprets its `data` argument as `sccp_msg_info_t`. Pascal removed the unsafe forwarding and calls the DTAP dissector with `NULL` instead. Release-3.6 !5951 and release-3.4 !5952 carry the accepted fix.

**Dispatch rule:** before forwarding a dissector `data` pointer, verify that the source and destination agree on the exact context type and lifetime. If the child supports absence of context and no compatible object exists, pass `NULL`; otherwise construct/obtain the context type the child actually requires.

**Review rule:** treat `call_dissector*()` data arguments as API contracts even though the compiler sees `void *`. A cast-free call is not evidence of type compatibility.

**Confidence:** Very high. Direct review caught a concrete cross-dissector type mismatch before merge, and the corrected master change was backported to both maintained branches.
