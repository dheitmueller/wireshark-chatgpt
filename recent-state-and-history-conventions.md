# Wireshark Recent-State and Active-Configuration Conventions

This file records durable conventions for state that is remembered for convenience versus state that should actively affect startup/runtime behavior. Current upstream source remains authoritative.

## Keep recent-history state separate from active configured state

A value being remembered because the user used or discovered it previously does not imply that Wireshark should automatically activate, reconnect to, probe, or otherwise act on it at the next startup.

Merged master MRs !13343 and !13353, authored by John Thacker, correct remote-capture behavior where hosts from `recent_common` were pre-populated and contacted when Manage Interfaces opened. That turned a convenience/history list into an implicit startup connection list, causing timeouts and problems for hosts requiring authentication. The accepted design keeps recent hosts as history/suggestions and stores the explicitly active remote hosts separately; !13353 uses structured preference data for the active host records and explicitly avoids persisting passwords.

**Implementation rule:** model recent/history state and active configuration as different semantic domains. Recent state can seed suggestions and UI history, but actions with network, privilege, authentication, latency, or resource consequences should be driven only by explicit active configuration. Do not infer startup intent from mere historical presence.

**Persistence rule:** when active configuration needs richer per-entry data or is expected to evolve, prefer a structured representation that can grow without overloading the recent-history format. Sensitive credentials should not be persisted merely because other connection metadata is persisted.

**Confidence:** Very high. Two merged master changes by John Thacker establish the split and its user-visible failure mode.

## Store bounded recent-history lists in the order that persistence actually consumes

When only the first N entries of a recent-history list are written, the in-memory ordering must make those first N entries the intended N most-recent values. Otherwise a bounded history can silently freeze on old entries even though new entries continue to be added in memory.

Merged release MRs !13326, !13327, and !13328 carry the capture-filter history fix to supported branches. The old list kept newer filters at the end while `recent_common` writing started at the beginning and stopped at the configured maximum, so after enough use the same oldest entries were repeatedly persisted. The accepted implementation keeps the newest filter first by prepending new entries, reverses the list once after reading the persisted file, and then lets both display and writing consume the list in natural order. The implementation also notes that if the maximum becomes preference-controlled, the recent file is read before that preference, so input must not be truncated before the effective limit is known.

**Implementation rule:** define MRU ordering explicitly and keep in-memory, presentation, and serialization order consistent. If a persistence limit is loaded later than the data itself, preserve enough input to apply the final limit after configuration is known rather than truncating under a default that may be wrong.

**Performance note:** for `GList`, repeated prepend followed by one reverse is O(N), while repeatedly appending N items is O(N²); the accepted implementation aligns the efficient data-structure operation with the desired MRU semantics.

**Confidence:** High. The same accepted correction was carried to three release branches and documents both the semantic and complexity rationale.