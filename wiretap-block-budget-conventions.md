# Wireshark Wiretap Block-Budget Conventions

This file records durable conventions for length-delimited wiretap/container parsing extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Consume length-delimited blocks through a checked remaining-byte budget

For a container or capture-file block with a validated total content length, initialize a `remaining` value once and treat every subsequent read as consumption from that budget. Before reading each fixed or variable component, use checked subtraction (for example `ckd_sub()`) to prove the component fits; reject malformed input before performing the read. Do not repeatedly derive independent subtraction expressions from the original total, because one missed underflow check can turn malformed lengths into a very large unsigned remainder.

Merged master MR !24236, authored and merged by Guy Harris and approved by John Thacker, rewrote the pcapng-sysdig event-block reader around `block_remaining`. Each part of the block is checked against the remaining budget before it is read, and construction of the completed `wblock` record is deferred until all required input has been read and validated. Guy explicitly noted in the MR discussion that this is a pattern worth applying to other pcapng block-reading code so that the compiler can implement the checked subtraction efficiently and the checks are systematically present.

Earlier merged !24224 and !24211 provide useful evolutionary evidence: they added individual underflow/sanity checks, but !24236 generalized the design into the remaining-budget model.

**Implementation rule:** validate the enclosing length once; then for each consumed item, checked-subtract its exact size from the remaining budget before reading it. Treat failure as malformed input with format-specific diagnostics.

**Publication rule:** when possible, do not populate externally visible record state incrementally while the block is still only partially validated. Read and validate the block first, then publish the completed record state.

**Confidence:** Very high. Merged master change authored and merged by Guy Harris, approved by John Thacker, with explicit maintainer commentary identifying the approach as a reusable pattern.
