# Wireshark Logging Conventions

This file records durable source-layout and logging conventions extracted from accepted Wireshark merge requests. Current upstream source remains authoritative.

## Define `WS_LOG_DOMAIN` immediately after the leading `config.h` include

For a source file that sets a Wireshark logging domain, include `config.h` first and define `WS_LOG_DOMAIN` on the immediately following line, before other project or system headers. This makes the compilation configuration and the translation unit's logging identity unambiguous before logging headers or code are pulled in.

Merged MR !23936 applies this layout consistently across 95 files: `config.h` is the first application include and `WS_LOG_DOMAIN` is placed on the next line. The change was authored by Jaap Keuter and merged by John Thacker.

**Implementation rule:** in source files with a custom logging domain, start with `#include "config.h"`, then `#define WS_LOG_DOMAIN ...`, then the remaining includes. Do not separate the log-domain definition from that setup with unrelated headers.

**Confidence:** High. Broad merged consistency change accepted on master; `config.h`-first is independently corroborated by later dissector reviews.

## Do not construct expensive diagnostics when the log message will be discarded

Debug logging should use Wireshark's logging framework and should avoid formatting, allocating, or traversing data solely for a message whose domain/level is inactive. When diagnostic construction is nontrivial, test `ws_log_msg_is_active()` (or the current equivalent) before doing that work.

Merged master MR !20587, authored and merged by John Thacker, converts OID/MIB diagnostics from a bespoke environment-variable/`printf` mechanism to `ws_log`. It also wraps expensive string creation in `ws_log_msg_is_active()` checks so disabled debug logging no longer pays to build strings that will never be emitted.

**Implementation rule:** prefer the project logging subsystem over private debug knobs/output paths, and treat diagnostic argument construction as real runtime work. Keep cheap scalar arguments inline, but guard allocations, string assembly, or expensive calculations whose only consumer is an inactive debug/trace message.

**Confidence:** Extremely high. Merged master cleanup authored and merged by John Thacker, with both the logging unification and avoided construction stated in the change.

## Distinguish recoverable API misuse from a fatal fuzzer finding

A precondition check that deliberately returns a safe value can indicate a programmer error without necessarily leaving the program in an inconsistent state. Making every such event fatal under generic `critical` logging can turn benign/recoverable argument handling into repetitive fuzzer aborts, obscuring actionable crashes.

Merged master MR !10332 changes the `ws_return.h` warning helpers from generic critical logging to informational events in the dedicated `InvalidArg` domain. Developers can still choose to make that domain fatal when investigating invalid calls, while normal fuzzing avoids unconditional termination for a helper that is explicitly documented as recoverable.

**Implementation rule:** assign severity according to whether the failed precondition leaves the state unsafe. Keep recoverable early-return diagnostics identifiable by a dedicated logging domain, and permit targeted fatal configuration for debugging instead of promoting all argument warnings to globally fatal errors.

**Confidence:** High — merged wsutil logging contract change with its explicit fuzzing/debugging rationale.


## Use build-elidable debug logging for hot-path diagnostics

Merged master MR !9913, authored by John Thacker and merged by Gilbert Ramirez, changes several `dfilter_compile_real()` debug-only messages from `ws_log(WS_LOG_DOMAIN, LOG_LEVEL_DEBUG, ...)` to `ws_debug(...)`. The MR explicitly states the objective: these calls should be optimized away in release builds.

**Implementation rule:** when debug logging has no production functional side effect, use the project's intended compile-time-elidable debug helper where available rather than assuming a runtime log-level filter also removes the formatting and call overhead. Never put observable behavior or required error propagation behind a debug-only macro.

**Confidence:** High. Merged maintainer-authored change with a documented release-build performance rationale; verify current macro behavior in the target branch.
