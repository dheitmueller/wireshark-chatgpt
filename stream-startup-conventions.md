# Wireshark Stream Startup Conventions

This file records durable conventions for buffered streams, FIFOs, pipes, and other producer/consumer startup handshakes. Current upstream implementation remains authoritative.

## Flush startup framing before waiting for payload or peer progress

When a producer writes format-identifying bytes or other startup framing through a buffered `FILE *`, those bytes are part of the protocol handshake with the consumer. If the consumer must see them before it can finish opening the stream, explicitly flush them before doing later work or waiting for payload. Do not accidentally make peer startup depend on the arrival of the first data packet merely because stdio has not yet emitted the header.

Merged master MR !11601, authored by John Thacker, fixes `udpdump` and `ciscodump` by calling `fflush()` immediately after writing the pcap header. Without the flush, dumpcap could remain blocked in `cap_pipe_open_live()` waiting for the header until an extcap packet finally arrived. The corrected behavior lets dumpcap finish opening even when no packets ever arrive; a valid header-only capture file is an acceptable result.

Guy Harris gave this change particularly strong review weight. He noted that tcpdump's packet-buffered mode likewise flushes after the capture file is initially created, stated that there was no reason extcaps should behave differently, approved the change, and suggested that the common pcap-header writer itself might eventually own pipe flushing. John agreed that centralizing the behavior could help other tools. Merged release-4.0 backport !11602 preserves the same startup contract with Guy as committer/approver. The immediately preceding review batch also contained merged !11655, which independently flushes a pcap stream after emitting its header.

**Implementation rule:** identify the earliest bytes a peer requires in order to recognize or open a stream. When those bytes are written through a buffering layer, make their visibility an explicit part of the producer's startup contract rather than an incidental consequence of a later full buffer or payload write.

**Compatibility rule:** do not preserve fragile launch-order behavior that only worked because startup framing was accidentally delayed in a userspace buffer. If making the handshake deterministic exposes an invalid FIFO/named-pipe ordering, prefer the deterministic protocol contract and document the required ordering.

**Testing rule:** include a zero-payload startup case where practical. A producer that writes a valid header and then receives no packets should still allow its consumer to finish initialization rather than blocking indefinitely waiting for buffered metadata.

**Confidence:** Extremely high. The rule is supported by a merged master fix from John Thacker, explicit approval and architectural commentary from Guy Harris, a merged stable backport, and an independent neighboring merged fix with the same flush-at-header boundary.