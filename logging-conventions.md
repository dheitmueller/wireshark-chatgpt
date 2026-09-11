# Wireshark Logging Conventions

This file records durable source-layout and logging conventions extracted from accepted Wireshark merge requests. Current upstream source remains authoritative.

## Define `WS_LOG_DOMAIN` immediately after the leading `config.h` include

For a source file that sets a Wireshark logging domain, include `config.h` first and define `WS_LOG_DOMAIN` on the immediately following line, before other project or system headers. This makes the compilation configuration and the translation unit's logging identity unambiguous before logging headers or code are pulled in.

Merged MR !23936 applies this layout consistently across 95 files: `config.h` is the first application include and `WS_LOG_DOMAIN` is placed on the next line. The change was authored by Jaap Keuter and merged by John Thacker.

**Implementation rule:** in source files with a custom logging domain, start with `#include "config.h"`, then `#define WS_LOG_DOMAIN ...`, then the remaining includes. Do not separate the log-domain definition from that setup with unrelated headers.

**Confidence:** High. Broad merged consistency change accepted on master; `config.h`-first is independently corroborated by later dissector reviews.
