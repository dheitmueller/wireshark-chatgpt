# Wireshark Capture Block Size-Validation Conventions

This file records durable conventions for applying resource and record-size limits to capture-container blocks. Current upstream source and capture-format specifications remain authoritative.

## Apply a size limit only to the semantic object it is intended to bound

A limit named or designed as a maximum packet/record size should not automatically be applied to every enclosing container block. Metadata, name-resolution, interface-description, statistics, or other non-packet blocks can have different legitimate size domains even when they share the same framing header. Validate the block type first, then apply the limit whose semantics match that type.

Merged master MR !15281 fixes dumpcap's pcapng pipe reader. `cap_pipe_max_pkt_size` is intended to constrain blocks that become packet/event/log records, but the old check rejected any pcapng block whose total length exceeded that packet-oriented limit. The accepted implementation introduces an `is_data_block()` classification and applies the maximum only to Enhanced Packet, Simple Packet, Packet, Systemd Journal Export, and Decryption Secrets blocks that actually feed record data; metadata blocks are no longer incorrectly constrained by a packet-size policy.

**Implementation rule:** before applying a numeric safety limit, identify the semantic object that the limit protects. Do not reuse a packet payload ceiling as a generic container-block ceiling merely because the same length field is convenient to inspect.

**Review rule:** for container formats with heterogeneous block types, test at least one large valid metadata/control block and one over-limit data block. The former should remain accepted while the latter is rejected according to the intended resource policy.

**Confidence:** Very high. Merged master capture-path correctness fix with the mismatch between pcapng block semantics and the packet-size limit made explicit in the accepted implementation.

## Bound each block parser to the block's declared data extent

Merged master MR !3247, authored by Guy Harris, reworked the pcapng file dissector so each block-type parser receives a tvbuff restricted to that block's data portion. A `ReportedBoundsError` inside that bounded view then identifies a block whose declared length is too short, and the dissector reports the structural defect on the block-length item. The same change checks that the trailing block length matches the leading block length and updates file-format tests to validate both values.

**Implementation rule:** give a nested/container parser a bounded view of the semantic object it owns rather than the remainder of the enclosing file. Let normal bounds handling detect attempts to cross the declared object boundary, then translate that failure into the format-specific structural diagnostic.

**Testing rule:** when a container repeats or cross-checks length metadata, validate the redundant framing value as well as the decoded content.

**Confidence:** Extremely high. Merged master parser architecture and tests authored by Guy Harris.
