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

## Separate normal diagnostic severity from validation strictness with log domains

A diagnostic can be too noisy to emit as a warning during ordinary packet analysis while still being valuable as a hard failure under fuzzing or targeted validation. Use a dedicated log domain so those two policies can be selected independently instead of globally raising the log level.

Merged master MR !8284, authored by João Valverde, moves UTF-8 contract diagnostics into a dedicated `UTF-8` domain at debug level because malformed text was still sufficiently common that a global warning was excessive. Merged !8286 adds fatal-domain selection, and merged !8291 makes a configured fatal domain active even when normal log-level/domain filtering would otherwise suppress it. Later reviewed fuzz work builds directly on this capability by making UTF-8 contract violations fatal in fuzz runs.

**Implementation rule:** give semantically important diagnostics a stable domain. Choose an operationally appropriate default severity, then let fuzz/CI/debug configurations promote selected domains to fatal when violating that contract should stop the run.

**Review rule:** do not solve validation visibility by making a noisy diagnostic globally severe. Ask whether a dedicated domain plus targeted fatal policy provides stronger tests with less normal-runtime noise.

**Confidence:** Very high. Three merged master logging changes by João Valverde establish the mechanism and intended policy.

## Command-line tools should use the shared logging subsystem instead of private debug counters

A tool-specific debug flag creates its own severity scale, help text, output routing, and interaction with quiet mode. When the project has a common logging framework, map diagnostics onto that framework rather than maintaining a parallel interface.

Merged master MR !5642, authored by John Thacker, removes text2pcap's repeatable `-d` option and maps its former levels to the standard DEBUG and NOISY levels. The tool exposes the common logging usage, its tests and release notes are updated, and `-q` is narrowed to suppressing normal option/packet-count summaries rather than forcibly changing the diagnostic log level.

**CLI rule:** distinguish ordinary program output suppression from diagnostic severity/filtering. A “quiet” switch for summaries is not automatically a replacement for the logging subsystem's level/domain controls.

**Migration rule:** when retiring a user-visible private debug option, document the equivalent shared logging levels and update tests/help in the same change.

**Confidence:** Extremely high. Merged master change authored by John Thacker.

## Do not attach incidental reporting-site provenance to deferred failures

Merged master MR !3404, authored by Guy Harris, changes dissector-bug reporting from `ws_warning()` to an explicit `ws_log()` call that does not add the source file, source line, and function of the generic exception handler. Those coordinates describe where the deferred message is emitted, not where the dissector bug occurred, and therefore add misleading rather than useful provenance.

**Implementation rule:** source-location metadata is useful only when it identifies the operation that failed. If an exception or saved diagnostic is emitted later from a common wrapper/catch site, preserve the original semantic context and omit generic reporting-site coordinates that would be identical for every failure.

**Confidence:** Extremely high. Merged master logging correction authored by Guy Harris.

## Early corroboration: guard expensive runtime diagnostics

Merged master MR !3393, shaped by João Valverde review, keeps dot11decrypt debug logging available through the runtime logging system rather than locally compiling it out. When João noticed that the debug dump allocates/formats memory, he requested an early `ws_log_message_is_active()` check; the accepted implementation adds it. The contributor also benchmarked roughly 800,000 encrypted frames and found no meaningful penalty once inactive work was avoided.

This is early independent corroboration of the later, stronger inactive-message construction rule already recorded in this file.

**Confidence:** High. Merged implementation with direct maintainer review and a workload-based performance check.

