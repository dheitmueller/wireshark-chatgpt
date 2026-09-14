# Wireshark Configuration and Runtime-State Conventions

This file records durable conventions for separating immutable configuration from per-run mutable state. Current upstream source remains authoritative.

## Keep configuration immutable during runtime processing

Parsed configuration describes policy and structure; mutable counters, indexes, caches, and instantiated objects belong to runtime state. Do not reuse configuration objects as convenient storage for per-capture or per-run mutations when the two lifetimes and ownership models differ.

Merged master MR !20475, authored by John Thacker and approved/merged by Anders Broman, restructures MATE so runtime processing no longer changes configuration structures. Per-GOP counters and hash tables move into `mate_runtime_data`, while runtime objects retain `const` pointers back to their configuration. The MR explicitly describes the separation as cleaner, easier to free correctly, and friendlier to a future move to wmem-managed memory.

**Implementation rule:** configuration-derived objects should be treated as read-only once parsing/initialization is complete. Store mutable IDs, indexes, caches, and instantiated protocol state in an object whose lifetime matches the processing run, and use `const` references from runtime objects back to configuration where practical.

**Ownership rule:** make cleanup follow the same boundary. Configuration cleanup owns configuration allocations; runtime cleanup owns runtime hash tables, indexes, counters, and instantiated objects. Avoid hidden cross-lifetime mutation that forces one cleanup path to understand another subsystem's state.

**Review rule:** when a configuration structure gains a mutable field, ask whether the value describes configuration or merely records what has happened while processing the current capture/session. If it is the latter, prefer explicit runtime state even if putting it in the configuration object would require fewer parameters today.

**Confidence:** Very high. The separation and ownership rationale are explicit in a merged master MR authored by John Thacker and approved/merged by Anders Broman.