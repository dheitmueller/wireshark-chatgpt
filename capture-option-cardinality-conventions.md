# Capture Option Cardinality Conventions

This file records durable Wiretap/pcapng option-cardinality conventions extracted from upstream Wireshark merge-request history. Current upstream source remains authoritative.

## Cardinality belongs to the format; add and set have different semantics

Merged master MR !3409, authored by Guy Harris, corrects an ERF comment that had conflated Wireshark implementation behavior with the pcapng specification. Most pcapng options permit only one instance per block, while some classes such as comments, interface addresses, packet hashes, and custom options may repeat.

For a single-instance option, `wtap_block_add_*_option()` is the create operation and fails when the instance already exists; `wtap_block_set_*_option()` changes an existing instance and fails when none exists. For repeatable options, add appends another instance, while `wtap_block_set_nth_*_option()` changes a particular existing occurrence.

**Implementation rule:** choose add, set, and set-nth from the option's specification-defined cardinality and the intended create-versus-update operation. Do not treat the APIs as interchangeable ways to assign a value.

**Review rule:** when code accumulates metadata over time, determine whether the target option is single- or multi-instance before deciding whether later data replaces an existing value, creates another value, or must be represented another way.

**Confidence:** Extremely high. The behavior and rationale are documented directly in merged master code by Guy Harris.
