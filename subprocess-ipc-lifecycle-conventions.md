# Wireshark Subprocess IPC and Lifecycle Conventions

This file records durable conventions for Wireshark parent/child process communication and file-descriptor lifetime. Current upstream source remains authoritative.

## Mark private helper pipes close-on-exec unless inheritance is part of the protocol

A pipe remains open as long as any process still owns a duplicate of either relevant file descriptor. Closing the parent's intended endpoint is therefore not sufficient to signal EOF if a subsequently forked-and-execed process inherited an unrelated copy. Private helper-process IPC descriptors should be marked close-on-exec unless the child being execed is explicitly meant to participate in that IPC channel.

Merged master MR !13860, authored and merged by John Thacker, fixes a Wireshark hang involving the MaxMind `mmdbresolve` helper. Wireshark expected `mmdbresolve` to exit after its pipe was closed, but a later `dumpcap` fork/exec inherited the pipe descriptors. The inherited copies kept the pipe alive, so `mmdbresolve` did not exit and Wireshark hung waiting in `waitpid()`. The accepted fix sets `FD_CLOEXEC` on the helper pipe descriptors after `mmdbresolve` is spawned so unrelated later execs cannot extend their lifetime.

**Implementation rule:** when creating IPC descriptors for a specific helper, make inheritance policy explicit. Prefer close-on-exec for descriptors that are private to the current parent/helper relationship, and deliberately clear or omit it only for descriptors that an execed child is intended to receive.

**Review rule:** for hangs around pipe EOF, helper shutdown, or `waitpid()`, audit every process that can inherit the descriptor, not just the two endpoints that conceptually own the channel. A leaked duplicate in an unrelated descendant changes pipe-lifetime semantics even if that descendant never reads or writes it.

**Testing rule:** exercise helper shutdown both before and after spawning other subprocesses that use fork/exec. Verify that disabling/stopping the helper reaches EOF and completes without waiting for an unrelated child to exit.

**Confidence:** Very high. Merged master lifecycle fix authored and merged by John Thacker with the inheritance mechanism and user-visible hang stated explicitly.

## Keep logging records distinct from command-failure records in subprocess protocols

Diagnostic logging and command-result signaling are different semantic channels even when they share one physical pipe. A log record may be DEBUG, INFO, or WARNING and must not accidentally tell the parent that an interface-list, statistics, or other control operation failed.

Merged master MR !13844, authored and merged by John Thacker, introduces a distinct `SP_LOG_MSG` record rather than sending child logs as `SP_ERROR_MSG`. The parent can then consume log records while continuing to wait for the actual success/error result. Merged master MR !13846 completes the design by passing the parent's effective Capchild log level to `dumpcap`, allowing runtime logging policy such as `--log-debug Capchild` to control child diagnostics without compile-time debug defines.

**Architecture rule:** define separate IPC record types for observability and operation status. A child log message should preserve its level and be routed through the parent's logger; success/failure records should remain reserved for control-flow semantics.

**Control-flow rule:** loops waiting for a child result must tolerate and process intervening log records without treating them as terminal success or failure. Continue until the protocol's actual terminal record arrives or transport failure occurs.

**Configuration rule:** where a parent owns user-facing logging policy, propagate the relevant effective log level/filter to the child rather than requiring separate compile-time switches or silently applying a different verbosity policy.

**Confidence:** Very high. Two adjacent merged master changes authored and merged by John Thacker establish both the record-type separation and runtime logging-policy propagation.

## Keep structured child IPC off ordinary diagnostic streams

A machine-readable parent/child protocol should use a dedicated channel rather than borrowing a conventional diagnostic descriptor such as standard error. Code outside the subprocess protocol's control, including third-party libraries, can legitimately write diagnostics to stderr and thereby corrupt framing if stderr is also being treated as structured IPC.

Merged master MR !13809, authored and merged by John Thacker, changes `dumpcap` so the sync pipe is no longer forced onto file descriptor 2. The parent instead passes the dedicated sync-pipe descriptor explicitly with `-Z`; the Windows path likewise carries the inherited sync-pipe handle explicitly and keeps its signal pipe as a separate option. The MR explains that stderr had become unsafe precisely because unrelated library or runtime diagnostics could appear there.

**Architecture rule:** give structured control/data IPC its own descriptor or handle. Keep stdout/stderr available for their normal text-output and diagnostic roles unless the subprocess interface explicitly defines those streams as the protocol and all writers are controlled.

**Process-launch rule:** when the child needs a nonstandard inherited descriptor or handle, communicate that endpoint explicitly as part of the launch contract rather than relying on a magic conventional descriptor number. Treat POSIX descriptor inheritance and Windows handle inheritance as platform-specific mechanisms implementing the same explicit logical channel.

**Testing rule:** deliberately emit ordinary child diagnostics while exercising the structured IPC path and verify that parent framing remains intact. Also test descriptor/handle passing independently from the protocol payload so launch-time inheritance failures are distinguishable from malformed messages.

**Confidence:** Very high. Merged master subprocess-protocol fix authored and merged by John Thacker; the corruption mechanism and accepted dedicated-channel design are stated directly in the change.