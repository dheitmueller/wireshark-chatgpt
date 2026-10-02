# Wireshark Process Lifecycle Conventions

This file records durable process-lifecycle conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## A long-running parent that forks per request must reap completed children

Creating a child process transfers lifecycle responsibility to the parent. If a long-running Wireshark component forks children and never waits for them, exited children can remain as zombies and eventually exhaust process-table or fork resources even though each individual child completed successfully.

Merged MR !23171 fixes sharkd's per-client fork model by draining already-exited children with non-blocking waitpid(-1, ..., WNOHANG) before creating another child. The loop continues until no completed child remains, avoiding both zombie accumulation and blocking on live children.

**Implementation rule:** every fork-based server path must have an explicit child-reaping strategy. For event-loop or request-driven parents, use a non-blocking reap path (or an equivalent SIGCHLD/event integration) that drains all completed children without waiting on children that are still running. Treat child cleanup as part of the resource lifecycle, not as optional housekeeping.

**Confidence:** High. Merged master correctness/resource fix, approved and merged by Anders Broman, for a concrete sharkd failure mode where unreaped children eventually prevented new forks.

## Centralize shared capture-pipe event handling

Merged master MR !7584, authored by Tomasz Moń, moves capture synchronization-pipe handling from separate Qt and CLI implementations into common capture code because both frontends already run a GLib main loop.

**Rule:** shared event-loop plumbing belongs in common capture/process code rather than duplicated frontend state.

**Confidence:** High. Merged master architecture change.

## Let the CLI use the event loop rather than emulate it with polling

Merged master MR !7558, authored by Tomasz Moń, has tshark actually run the GLib main loop during capture. On Unix, the synchronization fd is watched by the main loop. On Windows, timer callbacks execute in the main thread, so the callback mutex is removed, and child shutdown uses a bounded WaitForSingleObject call instead of repeatedly polling process state.

**Rule:** once a frontend participates in the common event-loop model, express readiness and lifecycle events through that model. Do not retain redundant locking or hand-written polling whose only purpose was to emulate event-driven behavior.

**Confidence:** High. Merged master capture/process architecture change.

## Prefer an explicit common extcap shutdown protocol over platform GUI discovery

Closed MR !2063 proposed graceful Windows extcap shutdown by enumerating top-level windows and posting `WM_CLOSE`. The implementation did not merge, so it is not an accepted code exemplar. Its review discussion is nevertheless useful architectural evidence: Guy Harris suggested a new controller/extcap mechanism using control pipes, Tomasz Moń also favored control-pipe shutdown, Gerald Combs said lifecycle handling should live behind a common extcap API, and reviewers objected to blocking waits and process-wide window enumeration.

**Architecture rule:** when a cooperating child needs graceful finalization, define shutdown in the child-process protocol and common lifecycle layer rather than inferring process control from GUI/window-system artifacts. Keep shutdown/event handling nonblocking with respect to the UI/event loop.

**Weighting note:** treat this as high-authority negative/design guidance, not as proof that one exact control-pipe protocol is current upstream policy, because !2063 was closed unmerged.

**Confidence:** Moderate-to-high. Unmerged implementation, but unusually strong cross-maintainer architectural convergence from Guy Harris, Tomasz Moń, Gerald Combs, and Graham Bloice, later consistent with event-loop work recorded above.
