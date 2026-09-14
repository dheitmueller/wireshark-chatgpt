# Wireshark Bounded Text Numeric Parsing Conventions

This file records durable conventions for converting bounded textual protocol/input fields to integers. Current upstream APIs remain authoritative.

## Parse bounded text in place with width-specific helpers

When numeric text already exists as a bounded byte span, prefer a parser whose interface accepts that span directly rather than allocating/copying it into a temporary NUL-terminated string for `strtoul()`, `g_ascii_strtoull()`, or similar C-library conversion. This matters for correctness as well as allocation cost: the source may contain internal NULs, the desired integer width may differ from C `long` on some platforms, and the parser should not accidentally inspect bytes beyond the protocol field.

Merged MR !24518, authored by John Thacker and merged by Anders Broman, introduces reduced-width `ws_buftou*()` variants and converts several dissectors away from allocate/copy/NUL-terminate parsing. Its release backports !24517 and !24513 preserve the counted-string semantics and tests. Merged MR !24524 adds `tvb_get_string_uint*()` helpers so dissectors can perform the same bounded conversion directly from tvbuff data, and merged !24557 adopts them in HTTP and Megaco. Merged !24528 demonstrates an additional portability benefit: replacing `strtoul()` avoids truncating 64-bit protocol fields on Windows, where `long` is 32 bits.

**Implementation rule:** keep the byte extent and numeric width explicit in the conversion API. Use `ws_buftou8/16/32/64` or the corresponding tvbuff helper when applicable; avoid manufacturing temporary C strings solely to call an unbounded or platform-width-dependent numeric parser.

**Confidence:** Very high. Merged master helper/API work by John Thacker, accepted backports, and multiple merged conversions showing both safety and portability benefits.

## Prefer Wireshark checked conversion helpers over generic C conversion routines

Even when input is already a conventional NUL-terminated string, prefer the Wireshark conversion helper that matches the intended integer width when it provides the project's expected syntax, range, and error checks. Generic C-library conversion can leave each caller to rediscover overflow, range, end-pointer, and error handling.

During review of merged MR !20031, Guy Harris specifically recommended replacing local numeric conversion with `ws_strtou32()` from `wsutil/strtoi.h` because it performs additional error checking, including range checking. The suggestion was incorporated before the XML parser migration was merged.

**Implementation rule:** select numeric parsers by the semantic width and validation contract of the destination value. Prefer `ws_strtou*()`/related Wireshark helpers over raw `strtoul()`-style calls when the project helper expresses the required range and error semantics.

**Confidence:** Extremely high. Direct Guy Harris review on a merged master parser migration, with the recommendation adopted before merge.