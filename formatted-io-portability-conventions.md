# Formatted-I/O Portability Conventions

Merged master MR !5498 moved epan formatting/parsing code toward the C library and `<inttypes.h>` format macros. Stig Bjørlykke caught a scanning call using the wrong family and requested `SCNx64` for `sscanf()`; João Valverde agreed and corrected it.

**Rule:** use `PRI*` macros with printf-family output and `SCN*` macros with scanf-family input. Treat input and output format strings as different contracts during mechanical migrations.

Merged master MR !5497 also exposed a macOS-only missing declaration after the formatted-string API migration. João traced it to a header dependency hidden by conditional compilation, and merged follow-up !5503 fixed it.

**Rule:** after broad API/header migrations, compile representative supported platform configurations that activate touched conditional paths. A clean common-platform build does not prove platform-only include/declaration correctness.

**Confidence:** Very high. Both lessons come from merged master work, with direct Stig Bjørlykke review and a concrete supported-platform regression/follow-up.
