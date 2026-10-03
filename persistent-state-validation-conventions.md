# Wireshark Persistent State Validation Conventions

This file records rules for validating packet-derived values that survive beyond the packet in which they were decoded.

## Packet-derived configuration remains untrusted after it is stored

Merged master MR !300 added the ILDA Digital Network dissector. During review, Martin Mathieson fuzzed configuration messages and showed that corrupted configuration could be accepted into state and then fail in a later packet. One supplied reproducer drove a fuzzed zero `sample_size` into `dissect_idn_laser_data()`, causing an integer divide-by-zero. The accepted revision restored a `sample_size` validity check.

**Implementation rule:** validating the current packet's immediate reads is insufficient when decoded values become cross-packet state. Before committing packet-controlled configuration, validate every invariant later consumers rely on. At use sites with severe consequences—division, allocation sizing, indexing, shift counts, or loop bounds—defensive revalidation is appropriate when state may have originated from malformed traffic.

**Fuzzing rule:** for stateful dissectors, fuzz the packet that establishes or mutates state and then continue dissection into packets that consume that state. A malformed setup packet can create failures that only occur later.

**Confidence:** Very high. Merged master new-dissector review with a concrete fuzz reproducer and a crash fixed before merge.
