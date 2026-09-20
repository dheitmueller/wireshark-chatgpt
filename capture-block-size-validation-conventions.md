# Wireshark Capture Block Size-Validation Conventions

This file records durable conventions for applying resource and record-size limits to capture-container blocks. Current upstream source and capture-format specifications remain authoritative.

## Apply a size limit only to the semantic object it is intended to bound

A limit named or designed as a maximum packet/record size should not automatically be applied to every enclosing container block. Metadata, name-resolution, interface-description, statistics, or other non-packet blocks can have different legitimate size domains even when they share the same framing header. Validate the block type first, then apply the limit whose semantics match that type.

Merged master MR !15281 fixes dumpcap's pcapng pipe reader. `cap_pipe_max_pkt_size` is intended to constrain blocks that become packet/event/log records, but the old check rejected any pcapng block whose total length exceeded that packet-oriented limit. The accepted implementation introduces an `is_data_block()` classification and applies the maximum only to Enhanced Packet, Simple Packet, Packet, Systemd Journal Export, and Decryption Secrets blocks that actually feed record data; metadata blocks are no longer incorrectly constrained by a packet-size policy.

**Implementation rule:** before applying a numeric safety limit, identify the semantic object that the limit protects. Do not reuse a packet payload ceiling as a generic container-block ceiling merely because the same length field is convenient to inspect.

**Review rule:** for container formats with heterogeneous block types, test at least one large valid metadata/control block and one over-limit data block. The former should remain accepted while the latter is rejected according to the intended resource policy.

**Confidence:** Very high. Merged master capture-path correctness fix with the mismatch between pcapng block semantics and the packet-size limit made explicit in the accepted implementation.