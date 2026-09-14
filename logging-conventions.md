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