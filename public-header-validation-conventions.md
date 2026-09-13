# Wireshark Public-Header Validation Conventions

This file records durable conventions for validating public/installable Wireshark headers. Current upstream source, packaging, and CI remain authoritative.

## Test shipped public headers as consumer-facing compilation units

A header that is installed for consumers must be usable from the source-language modes it claims to support without depending on incidental include order or on private build context. Wireshark itself can compile successfully while a shipped header is unusable by an external C consumer if another internal header happened to provide declarations first, or if C++-only constructs leak into a header intended for C.

Merged master MR !21482 adds a packaging-time check for shipped headers after a public include was found not to work from pure C. The accepted implementation generates inclusion of the installed header set on the fly rather than maintaining a parallel hand-written header-check source. Guy Harris was the designated reviewer and Anders Broman merged the change. The immediate c128 header corrections in !21445/!21495 provide the concrete failure mode that motivated making the consumer contract mechanically testable.

**Implementation rule:** mechanically include/preprocess installed public headers from a clean consumer context in each supported language mode, rather than relying on Wireshark's internal translation units to prove header usability. Generate the test input from the authoritative shipped-header list where practical so newly installed headers enter the check automatically.

**Confidence:** Very high. Merged master build/packaging validation with Guy Harris review assignment and Anders Broman merge, following an actual public-header C-compatibility failure.
