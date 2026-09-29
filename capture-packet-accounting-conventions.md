# Wireshark Capture Packet-Accounting Conventions

This file records durable conventions for packet counters and capture-loop accounting. Current upstream capture code remains authoritative.

## Update counters at the semantic event boundary

Merged master MR !4616, authored by Guy Harris, moves packet-captured, packet-written, and sync-pipe accounting into `capture_loop_wrote_one_packet()`. Before the change, different capture paths incremented the same logical counters after bulk reads or queue dequeues, making it easy for threaded and non-threaded paths to drift.

Adjacent “double received count when using threads” fixes show the concrete failure mode: a source-level receive counter could be incremented by the threaded path and again by common packet-written code. In !4616 the per-source `received` increment is explicitly limited to the non-threaded case while common “one packet was successfully written” counters live in the one-packet completion function. Stable backports !4618, !4623, and !4631 carry the same centralization.

**Implementation rule:** if a counter represents a committed semantic event such as “one packet written,” update it at the function that actually commits that event. Do not duplicate the increment in every producer/read/dequeue path that might lead to it.

**Concurrency rule:** distinguish counters owned by worker/source acquisition from counters owned by common post-processing. A shared completion helper must not re-count work already accounted for by a threaded producer.

**Review rule:** when changing threaded capture accounting, enumerate every path that can increment each counter and verify that one real packet causes exactly one increment in the intended domain, for both threaded and non-threaded paths.

**Confidence:** Extremely high. Merged master restructuring authored by Guy Harris, with multiple merged stable-branch backports and adjacent double-count fixes.
