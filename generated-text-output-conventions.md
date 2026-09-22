# Wireshark Generated Text Output Conventions

This file records durable conventions for Wireshark features that generate text intended to be consumed as source code or another formal language. Current upstream source remains authoritative.

## Generate text according to the target language's lexical grammar

Output advertised as source code must be valid according to the target language, not merely visually plausible. Escape choices must account for how the target lexer determines where an escape ends and what adjacent bytes can change its meaning.

Merged master MR !15128, authored and merged by John Thacker, fixes Qt's “Copy Bytes as C String” output. The previous `\xNN` representation was unsafe because a C hexadecimal escape consumes an arbitrary number of following hexadecimal digits, so an unescaped hex-looking byte immediately after it could change the generated value. The accepted implementation escapes quote and backslash, uses the standard single-character C escapes where appropriate, and emits other bytes as fixed-width three-digit octal escapes, whose lexical extent is bounded. Release-4.2 MR !15132 carries the same fix.

**Implementation rule:** when exporting C, JSON, shell text, display filters, or another formal syntax, choose an escaping/serialization primitive whose token boundary is unambiguous for arbitrary following input. Include the language's delimiter and escape characters themselves in the audit; testing only non-printable bytes is insufficient.

**Submission rule:** a small correctness fix needed on a maintained branch need not be blocked on introducing a broader public helper API. Land the narrowly scoped grammar-correct fix when appropriate, then consider reusable API consolidation separately.

**Confidence:** Very high. The master fix was authored and merged by John Thacker, received positive maintainer review, and was subsequently carried to the maintained 4.2 branch.

## Serialize one logical field before applying delimiter-separated escaping

For CSV-like or delimiter-separated output, escaping belongs at the serialization boundary of the complete logical field. If a field consists of multiple values joined by an aggregator, compose that field first and then quote/escape the result once. Escaping each component independently can miss separators, quote characters, or aggregator bytes that only acquire syntax after composition.

Merged master MR !13111, authored by John Thacker, fixes TShark delimiter-separated field output by centralizing the grammar in `ws_escape_csv()`. The accepted path first joins repeated values with the configured aggregator and then escapes the complete field. When quoting is enabled, embedded quote characters are doubled according to the output grammar; when quoting is disabled, the field separator itself must be escaped. The same change updates the user documentation and exports the helper as a `libwsutil` symbol rather than keeping a second ad-hoc implementation in `epan/print.c`.

**Implementation rule:** separate semantic composition from syntactic serialization. Build the value that conceptually occupies one output field, then pass that complete value through the format-specific escaping routine using the actual separator, quote, and escape settings. Do not independently escape fragments and concatenate them afterward unless the target grammar explicitly defines fragment-level escaping.

**API rule:** when multiple producers need the same textual grammar, prefer one shared escaping primitive and make its parameter contract explicit. Update documentation, exported-symbol metadata, and representative tests together when that primitive becomes externally visible.

**Confidence:** Very high. Merged master correctness change authored by John Thacker; the diff directly aligns implementation, public helper API, documentation, and tests around the same serialization rules.