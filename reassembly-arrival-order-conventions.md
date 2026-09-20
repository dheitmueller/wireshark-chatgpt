# Wireshark Reassembly Arrival-Order Conventions

This file records durable conventions for fragment-length and completion decisions when valid protocol fragments can arrive out of order. Current upstream reassembly APIs and protocol specifications remain authoritative.

## Derive terminal-fragment payload from protocol geometry, not capture arrival order

A fragment that is logically the last fragment can arrive before earlier fragments. Reassembly must not treat “currently highest/last observed fragment” as proof that every byte carried in that frame is application payload, especially when terminal frames can contain padding. Otherwise an early terminal fragment can consume padding as data and cause later-arriving real payload to be truncated.

Merged master MR !15300 fixes exactly this failure in ISO 15765. Previously, when the last Consecutive Frame arrived out of order, the dissector used the entire frame including padding and subsequently truncated a late frame. The accepted implementation uses ISO 15765-2 framing rules to determine how many bytes each Consecutive Frame can actually contribute and tracks the logical last byte independently of packet arrival order.

**Implementation rule:** determine fragment contribution and terminal status from protocol-defined message length, sequence position, and fragment geometry. Do not infer payload length from whether a fragment happens to be the last one observed so far.

**Review rule:** for protocols allowing reordering, test a capture in which the logical final fragment arrives before one or more middle fragments, including a final fragment that contains padding. Verify both the reconstructed payload and the completion decision after the delayed fragments arrive.

**Confidence:** High. Merged master correctness fix with the out-of-order padded-final-fragment failure and the protocol-derived replacement algorithm documented directly in the MR.