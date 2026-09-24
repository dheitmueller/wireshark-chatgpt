# Pending Lifecycle State Conventions

Merged MR !10994 provides strong evidence that a semantically distinct pending phase should be represented explicitly rather than being treated as either completed or fully active.

**Implementation rule:** when an intermediate phase has different validity, ownership, or user-visible semantics from both neighboring phases, give it an explicit state and track pending work separately when necessary.

**Confidence:** Very high.
