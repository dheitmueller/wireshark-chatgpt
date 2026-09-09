# Wireshark Capture Diagnostic and Status Conventions

This file records durable conventions for capture-layer return status, warning propagation, and user-facing diagnostics. Current upstream source remains authoritative.

## Preserve warning versus failure semantics across capture-layer boundaries

A capture API can succeed while reporting that a requested property could not be honored. That state must not be flattened into a fatal error merely because it carries diagnostic text.

Merged MR !25732 was authored and merged by Guy Harris. It propagates libpcap activation warnings separately from errors through dumpcap's sync protocol and the capture-session callback interface. The motivating example is promiscuous-mode activation: capture can continue even when the requested mode was unavailable, so the GUI can present a warning instead of an error. The change intentionally does not surface equivalent warnings for unrelated operations where they are unlikely to occur or would add noise without actionable value.

**Implementation rule:** preserve success-with-warning, failure, and success as distinct semantic states through IPC and callback layers. Surface a warning where it changes what the user should know about the resulting capture; do not promote it to an error, and do not indiscriminately expose low-value warnings in every operation.

**Confidence:** Extremely high. Merged master capture architecture authored and merged by Guy Harris.

## Return success/failure directly when the callee owns the determination

Do not force callers to infer whether a helper succeeded by examining platform-specific side effects or conditionally compiled state when the helper already knows the answer.

Merged MR !25713, also authored and merged by Guy Harris, changes `cap_pipe_open_live()` and its format-specific helpers to return `bool`. This removes caller-side `#if` logic that previously inferred failure from different state on different platforms.

**Implementation rule:** expose the semantic result directly in the helper's return contract when it is universally meaningful. Keep detailed error text/state as supplemental diagnostics rather than making callers reconstruct the primary success/failure result from implementation details.

**Confidence:** Extremely high. Merged master cleanup authored and merged by Guy Harris.