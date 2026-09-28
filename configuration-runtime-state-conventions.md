# Wireshark Configuration and Runtime-State Conventions

This file records durable conventions for separating immutable configuration from per-run mutable state. Current upstream source remains authoritative.

## Keep configuration immutable during runtime processing

Parsed configuration describes policy and structure; mutable counters, indexes, caches, and instantiated objects belong to runtime state. Do not reuse configuration objects as convenient storage for per-capture or per-run mutations when the two lifetimes and ownership models differ.

Merged master MR !20475, authored by John Thacker and approved/merged by Anders Broman, restructures MATE so runtime processing no longer changes configuration structures. Per-GOP counters and hash tables move into `mate_runtime_data`, while runtime objects retain `const` pointers back to their configuration. The MR explicitly describes the separation as cleaner, easier to free correctly, and friendlier to a future move to wmem-managed memory.

**Implementation rule:** configuration-derived objects should be treated as read-only once parsing/initialization is complete. Store mutable IDs, indexes, caches, and instantiated protocol state in an object whose lifetime matches the processing run, and use `const` references from runtime objects back to configuration where practical.

**Ownership rule:** make cleanup follow the same boundary. Configuration cleanup owns configuration allocations; runtime cleanup owns runtime hash tables, indexes, counters, and instantiated objects. Avoid hidden cross-lifetime mutation that forces one cleanup path to understand another subsystem's state.

**Review rule:** when a configuration structure gains a mutable field, ask whether the value describes configuration or merely records what has happened while processing the current capture/session. If it is the latter, prefer explicit runtime state even if putting it in the configuration object would require fewer parameters today.

**Confidence:** Very high. The separation and ownership rationale are explicit in a merged master MR authored by John Thacker and approved/merged by Anders Broman.

## Initialize product configuration identity once and preserve the legacy default

Merged !6494 generalizes filesystem/config initialization to `configuration_init(argv[0], namespace)`; config directories, plugin/extcap locations, and environment-variable prefixes are then derived from the selected product namespace. Jim Young's macOS validation caught that the initial null/default handling broke ordinary Wireshark executables, and Gerald Combs corrected it.

**Initialization rule:** product/configuration identity should be established centrally and early, then consumed by generic path/configuration helpers. When generalizing an existing initialization API, preserve and test the legacy/default caller path in all ordinary executables, not only the new frontend.

**Confidence:** Very high. Merged Gerald Combs architecture with an immediately reproduced cross-application initialization regression and accepted fix.
