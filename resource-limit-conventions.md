# Resource Limit Handling Conventions

This file records durable Wireshark conventions for operations that can exceed process resource limits. Current upstream source remains authoritative.

## Degrade to a semantics-preserving bounded algorithm when a process limit is reached

An operation that is naturally simplest when all inputs are open at once should not necessarily expose the operating system's current soft resource limit as an application-level maximum. If the work can be decomposed into bounded batches without changing its intended semantics, use the limit as a trigger for the bounded algorithm rather than rejecting otherwise-valid input.

Merged master MR !12186, authored and merged by John Thacker, removes mergecap/libwiretap's practical open-file-count limit. `merge_open_in_files()` can stop successfully after opening the largest usable batch when `EMFILE` is reached, and the merge path then produces intermediate temporary files while preserving the ordering needed for the final merge. The MR explicitly calls out two metadata-order differences as pre-existing underlying issues rather than silently claiming byte-for-byte equivalence.

Guy Harris's review sharpened the accepted design: on UNIX-like systems, first try to raise the process's soft `RLIMIT_NOFILE` toward the requested need, bounded by the hard/platform limit, and only fall back to batching when that does not provide enough descriptors. John incorporated that direction. The implementation also limits the raise attempt to once before falling back, avoiding a possible retry loop on platforms where `setrlimit()` and the subsequently reported/effective limit do not behave identically. The discussion documents macOS/BSD differences around `OPEN_MAX`, `kern.maxfilesperproc`, and `setrlimit()` behavior rather than assuming one UNIX implementation.

**Implementation rule:** distinguish a recoverable process-resource ceiling from an intrinsic format/application limit. When an operation can be decomposed, fall back to bounded batching that preserves the operation's semantic ordering and output contract instead of imposing the current soft limit on users.

**Resource-policy rule:** when safe and unprivileged, a process may first attempt to raise its own soft limit, but cap the requested value against the platform's allowed maximum and treat failure or partial effectiveness as a normal fallback condition. Do not require privilege escalation merely to avoid batching.

**Termination rule:** bound resource-limit adaptation attempts. Platform APIs can report nominal limits differently from effective limits, so a successful `setrlimit()` call must not create an unbounded retry cycle if a subsequent open still reaches the limit.

**Review rule:** document any semantic differences introduced by batching or intermediate files, especially ordering and metadata aggregation. Separate pre-existing nondeterminism from changes caused by the new bounded algorithm.

**Confidence:** Extremely high. The merged master implementation was authored by John Thacker and materially revised in response to detailed Guy Harris review, including cross-platform resource-limit behavior.