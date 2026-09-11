# Wireshark Process Lifecycle Conventions

This file records durable process-lifecycle conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## A long-running parent that forks per request must reap completed children

Creating a child process transfers lifecycle responsibility to the parent. If a long-running Wireshark component forks children and never waits for them, exited children can remain as zombies and eventually exhaust process-table or fork resources even though each individual child completed successfully.

Merged MR !23171 fixes sharkd's per-client fork model by draining already-exited children with non-blocking `waitpid(-1, ..., WNOHANG)` before creating another child. The loop continues until no completed child remains, avoiding both zombie accumulation and blocking on live children.

**Implementation rule:** every fork-based server path must have an explicit child-reaping strategy. For event-loop or request-driven parents, use a non-blocking reap path (or an equivalent SIGCHLD/event integration) that drains all completed children without waiting on children that are still running. Treat child cleanup as part of the resource lifecycle, not as optional housekeeping.

**Confidence:** High. Merged master correctness/resource fix, approved and merged by Anders Broman, for a concrete sharkd failure mode where unreaped children eventually prevented new forks.
