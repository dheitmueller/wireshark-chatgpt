# Wireshark Source-Text Portability Conventions

This file records portability guidance for source-file character encoding.

## UTF-8 source is supported; non-ASCII remains a portability decision

Wireshark source files may use UTF-8. That does not mean every non-ASCII spelling or identifier is equally portable across the supported toolchain and runtime ecosystem.

Merged master MR !118 updates `README.developer` to permit UTF-8 source while advising that characters outside ASCII be used sparingly. The rationale notes that older GCC versions may have limitations around extended identifiers, editors can interpret text differently, and consoles—especially historical Windows command prompts—may have limited UTF-8 support. The same documentation records that most Wireshark strings and console output use UTF-8, while Qt APIs use UTF-16.

**Portability rule:** save source as UTF-8 and use non-ASCII deliberately. Prefer ASCII for identifiers and diagnostic/control surfaces where compiler, terminal, or tooling compatibility matters unless the non-ASCII character is semantically necessary.

**Boundary rule:** be explicit when crossing string-representation boundaries. Core Wireshark UTF-8 strings and Qt UTF-16 strings are different API domains even when the source file itself is UTF-8.

**Confidence:** Very high. Merged master developer-guidance change authored by Gerald Combs.
