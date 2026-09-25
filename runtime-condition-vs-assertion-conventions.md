# Runtime Condition vs Assertion Conventions

This file records durable conventions for distinguishing programmer invariants from packet-, preference-, and caller-dependent runtime states. Current upstream source remains authoritative.

## Do not assert on a state that normal inputs or supported configuration can reach

A dissector assertion should express an invariant whose failure indicates a programming error. If packet contents, checksum validation, user preferences, heuristic selection, capture truncation, or caller capabilities can legitimately make the condition false, use the corresponding normal error or exception path instead.

Merged master MR !9327, authored by Gerald Combs, fixes TCP desegmentation when a subdissector asks for another segment but TCP cannot perform reassembly. The old path asserted that desegmentation was enabled and possible. The MR explains ordinary reasons that assumption can fail, including checksum validation and disabled reassembly preferences, and replaces the assertion with `FragmentBoundsError`, the same semantic class used when requested PDU bytes are unavailable.

Gerald also notes that this reduces fuzzing noise: a packet-reachable assertion makes a malformed or context-limited input look like an internal invariant failure.

**Implementation rule:** assertions are for impossible internal states. Use TVBuff/bounds exceptions, expert information, ordinary failure returns, or other documented control flow for states that external input or supported configuration can produce.

**Review rule:** when a reproducer reaches an assertion, first determine whether the predicate is truly an internal invariant. If normal malformed, truncated, or configuration-dependent input can violate it, convert the path to the semantic runtime error.

**Confidence:** Very high. Merged master correctness change authored by Gerald Combs with explicit runtime rationale.
