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

## Derive transport buffer capacity from the IPC record-size contract

The maximum logical message size and the physical buffer used to receive that message are one invariant. Keeping them as unrelated constants creates a latent failure: increasing the protocol limit without increasing the receiving buffer can turn a valid message into truncation, memory corruption, or a crash.

Merged master MR !13745, authored by John Thacker, replaces duplicated fixed `PIPE_BUF_SIZE` definitions with `PIPE_BUF_SIZE (SP_MAX_MSG_LEN+4)`, explicitly accounting for the sync-pipe indicator/header in addition to the maximum payload. The MR states that changing `SP_MAX_MSG_LEN` without changing the old fixed buffer could lead to a segfault.

**Implementation rule:** derive receive-buffer sizes from the maximum framed-record size, including every header/indicator byte, or enforce the relationship with a compile-time assertion. Do not maintain a second magic constant that merely happens to be large enough today.

**Review/testing rule:** when changing an IPC maximum, framing header, or serialization size, review every allocation/read buffer on both sides of the protocol. Exercise records at and just below the maximum supported size.

**Confidence:** Very high. Merged master change by John Thacker that directly encodes the size relationship and documents the crash mode.

## Design capability IPC schemas for extension rather than today's fixed property set

Interface/capability discovery data is an evolving schema, not a permanently fixed tuple. A parent/child protocol should not hard-code assumptions that today's set of link types, addresses, options, and properties is exhaustive when the underlying capture API can grow.

Merged master MR !13743 integrates interface discovery and running statistics so one `dumpcap` child can populate the welcome screen and then continue delivering stats. During the substantial review discussion, Guy Harris explicitly cautioned against assumptions about the future set of per-interface options and characteristics: libpcap may expose extensible settable options and pcapng-style interface characteristics, and the representation should remain capable of carrying such additions. The surrounding !13738/!13720/!13715 series uses JSON serialization for interface/capability data.

**Architecture rule:** use an extensible structured representation for evolving capability/property sets, and make readers tolerate additional fields they do not yet understand. Avoid positional/fixed-width IPC shapes whose compatibility depends on the set of properties never changing.

**Review rule:** when adding a new capability to an IPC schema, ask whether the representation can naturally carry the next unknown capability as well. Treat upstream API extensibility as part of the compatibility design, not as a later migration problem.

**Confidence:** Extremely high. Merged master capture architecture series with direct, forward-looking review guidance from Guy Harris.

## Use real serialization for machine-readable records whose strings can contain delimiters

A line-oriented or delimiter-separated child protocol is only safe if the field grammar guarantees that payload strings cannot contain those delimiters. Interface names, descriptions, addresses, and other platform/user-provided text do not provide that guarantee, and escaping rules added later become an ad hoc serialization format of their own.

Merged master MR !13680, authored and merged by John Thacker, replaces `dumpcap`'s newline/tab-delimited machine-readable interface list with JSON. The MR explicitly cites embedded tabs in names/descriptions as a correctness problem and also uses the structured representation to prepare for combining interface, capability, and statistics discovery. The reader validates that returned data parses as JSON and treats malformed child output as an explicit error rather than continuing with a partially split record.

**Architecture rule:** when subprocess records contain unconstrained strings or an evolving collection of typed fields, use a structured serialization format with defined string escaping and field boundaries rather than inventing delimiter conventions.

**Validation rule:** parsing structured child output is part of the IPC trust boundary. Validate the document and expected field/token shapes, and report malformed output as a protocol failure instead of silently skipping or partially accepting records.

**Testing rule:** include names/descriptions containing tabs, newlines/escaping characters where the schema permits them, missing or wrong-typed fields, malformed serialization, and unknown additional fields. A format is not robust merely because the producer normally emits simple ASCII today.

**Confidence:** Very high. Merged master IPC change authored and merged by John Thacker, with both the delimiter failure and forward-extensibility motivation stated directly.