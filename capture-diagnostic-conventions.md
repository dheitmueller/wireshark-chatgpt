# Wireshark Capture Diagnostic and Status Conventions

This file records durable conventions for capture-layer return status, warning propagation, and user-facing diagnostics. Current upstream source remains authoritative.

## Preserve warning versus failure semantics across capture-layer boundaries

A capture API can succeed while reporting that a requested property could not be honored. That state must not be flattened into a fatal error merely because it carries diagnostic text.

Merged MR !25732 was authored and merged by Guy Harris. It propagates libpcap activation warnings separately from errors through dumpcap's sync protocol and the capture-session callback interface. The motivating example is promiscuous-mode activation: capture can continue even when the requested mode was unavailable, so the GUI can present a warning instead of an error. The change intentionally does not surface equivalent warnings for unrelated operations where they are unlikely to occur or would add noise without actionable value.

**Implementation rule:** preserve success-with-warning, failure, and success as distinct semantic states through IPC and callback layers. Surface a warning where it changes what the user should know about the resulting capture; do not promote it to an error, and do not indiscriminately expose low-value warnings in every operation.

**Confidence:** Extremely high. Merged master capture architecture authored and merged by Guy Harris.

## Keep consuming asynchronous IPC records until the command reaches a terminal response

A framed child-process protocol can carry informational or logging records while a synchronous command is still in progress. Receiving such a record does not mean that the command has completed.

Merged master MR !15927, authored and merged by John Thacker, fixes the dumpcap capture-sync command path after noisy logging could place an `SP_LOG_MSG` ahead of the command's `SP_SUCCESS` or `SP_ERROR_MSG`. The old code read one framed message and returned, which could make interface discovery fail or even lead to a crash. The accepted implementation processes the log record and continues reading complete sync-pipe frames until it reaches a terminal command result; EOF or a transport read failure remains a genuine terminal failure.

**Implementation rule:** in request/response IPC that permits asynchronous side-channel records, classify protocol messages as intermediate versus terminal. Consume and handle intermediate log/status messages without completing the transaction, and continue until the protocol-defined success/failure response or an actual transport failure arrives.

**Testing rule:** exercise command paths with logging/status chatter enabled so an intermediate record is deliberately interleaved before the terminal response. A quiet default run does not test the framing/control-flow contract.

**Confidence:** Very high. Merged master capture-process correction authored and merged by John Thacker, with an analogous existing loop cited in the MR rationale.

## Return success/failure directly when the callee owns the determination

Do not force callers to infer whether a helper succeeded by examining platform-specific side effects or conditionally compiled state when the helper already knows the answer.

Merged MR !25713, also authored and merged by Guy Harris, changes `cap_pipe_open_live()` and its format-specific helpers to return `bool`. This removes caller-side `#if` logic that previously inferred failure from different state on different platforms.

**Implementation rule:** expose the semantic result directly in the helper's return contract when it is universally meaningful. Keep detailed error text/state as supplemental diagnostics rather than making callers reconstruct the primary success/failure result from implementation details.

**Confidence:** Extremely high. Merged master cleanup authored and merged by Guy Harris.

## Preserve the semantic layer that actually failed

Error codes and user-facing diagnostics should identify the operation that failed rather than collapsing a lower-layer failure into a nearby but different action. This lets callers make correct decisions and prevents misleading messages such as reporting an encoding/compression failure as a raw file-write failure.

Merged MR !25509, authored and merged by Guy Harris, renames `FILE_ERR_CANT_WRITE` to `FILE_ERR_CANT_COMPRESS` for errors returned by compression libraries and uses that value consistently for compression-library failures. The output path may ultimately be writing a file, but the failed operation is compression, and the error domain records that distinction.

**Implementation rule:** translate errors at abstraction boundaries without erasing their semantic cause. Use a write error for an actual write failure, a compression error for compressor failure, and equivalent layer-specific statuses elsewhere; carry supplemental library/OS details separately when useful.

**Confidence:** Extremely high. Merged master file-wrapper error handling authored and merged by Guy Harris.

## Diagnose capture filters in the context that will actually execute them

Capture-filter validity and compiled BPF output can depend on the actual capture path, not just a nominal link-layer type. Linux live capture can expose libpcap BPF extensions unavailable to an offline/dead-handle compiler, and extcap interfaces have their own filter-verification mechanism. A GUI should therefore avoid presenting an approximation as the exact filter that the live capture path will use.

Merged MRs !21014 and !21025 add a dumpcap path for obtaining the compiled filter for a real capture interface and switch the Qt Compiled Filter Output dialog to that result. The !21025 discussion includes direct Guy Harris review distinguishing a libpcap filter expression from the resulting compiled BPF program and emphasizing the Linux live-capture target semantics. John Thacker separately called out extcaps, which dumpcap cannot validate as ordinary devices and for which `extcap_verify_capture_filter()` is the relevant mechanism.

**Implementation rule:** when reporting the exact compiled capture program or authoritative validity, use the same interface/context and backend that will execute the capture. If lightweight background syntax checking cannot safely or cheaply obtain that context—because it would require opening devices, crossing a privilege boundary, or invoking a different extcap validator—represent the result as context-dependent/indeterminate rather than falsely declaring it valid or invalid from an approximation.

**Confidence:** Extremely high. Accepted master capture/UI changes authored by John Thacker with substantive Guy Harris review of the expression-versus-program and live-target distinction.

## Propagate low-latency streaming semantics through every buffering layer

A user-visible promise such as line-buffered or immediate live output is an end-to-end contract. Flushing the final process is insufficient if an upstream capture process still intentionally batches records before delivering them over IPC.

Merged master MR !15406, authored by John Thacker and merged by Anders Broman, observes that TShark's `-l` option exists specifically so a downstream program or script can see each live-capture packet promptly. The accepted change therefore sets dumpcap's update interval to zero when `-l` is active, preventing dumpcap's normal update batching from defeating TShark's output-buffering choice.

**Implementation rule:** when an option promises low-latency streaming, audit all producer/consumer and IPC buffering stages involved in the path. Configure upstream batching consistently with the requested semantics rather than changing only the final stdout/stderr buffering policy.

**Confidence:** Very high. Merged master end-to-end live-capture behavior authored by John Thacker.

## Preserve the primary failure when cleanup or shutdown reports another error

Failure handling often performs a second operation—closing a helper command, tearing down IPC, or releasing a resource—that can itself produce an error. That secondary failure must not overwrite a more specific diagnostic that already explains why the requested operation failed.

Merged master MR !13987 fixes a capture path where the errno-derived message returned from dumpcap was overwritten by `sync_pipe_close_command()`, leaving callers without the useful reason for the original failure. The accepted change keeps the dumpcap diagnostic and intentionally ignores the less useful close-command message on that path. Guy Harris directly approved the MR, and John Thacker merged it.

**Implementation rule:** once a primary operation has failed, preserve its status and diagnostic across best-effort cleanup. If cleanup failure is itself important, report or log it separately; do not reuse the primary error slot in a way that replaces the root-cause message with a consequence of shutting the failed operation down.

**Review rule:** inspect error paths for helper calls made after the first failure. Out-parameters such as `err`, `err_str`, or secondary status objects are especially prone to accidental overwrite when reused for cleanup.

**Testing rule:** force both the main operation and the subsequent cleanup/close step into error-capable paths and verify that the user-facing message still describes the primary failure. Secondary diagnostics may be retained separately but must not erase the original cause.

**Confidence:** Extremely high. Merged master capture error-propagation fix with direct Guy Harris approval and John Thacker merge.

## Send machine-readable failure status across helper-process boundaries

When a parent process may need to react differently to distinct child/helper failures, the IPC contract should carry the structured failure identity rather than only preformatted user-facing text. Parsing prose to infer whether an operation failed because of permissions, missing files, or another errno is brittle and makes localization or wording changes part of the control-flow protocol.

Merged master MR !12020, authored and approved by Guy Harris, changes the capture child so an `exec` failure for dumpcap sends the `errno` value to the parent. This matters on systems where dumpcap is executable only by a capture-enabled group: an `EACCES` result can be identified directly instead of guessed from an error string. The parent remains responsible for formatting the user-facing message, and the structured status leaves room for more specific policy or guidance later.

**Implementation rule:** send a stable status/error domain over process or privilege boundaries when the receiver may branch on the cause. Format localized or user-oriented text at the layer presenting the error; carry raw supplemental text only as diagnostic detail, not as the sole machine-readable failure signal.

**Review rule:** if code is matching or heuristically parsing an error sentence emitted by another process in order to decide behavior, treat that as evidence that the IPC protocol is missing a structured status field.

**Confidence:** Extremely high. Merged master capture-process error-contract change authored and approved by Guy Harris, with the permission-denied use case stated explicitly in the MR rationale.
