# Wireshark Generated Text Output Conventions

This file records durable conventions for Wireshark features that generate text intended to be consumed as source code or another formal language. Current upstream source remains authoritative.

## Generate text according to the target language's lexical grammar

Output advertised as source code must be valid according to the target language, not merely visually plausible. Escape choices must account for how the target lexer determines where an escape ends and what adjacent bytes can change its meaning.

Merged master MR !15128, authored and merged by John Thacker, fixes Qt's “Copy Bytes as C String” output. The previous `\xNN` representation was unsafe because a C hexadecimal escape consumes an arbitrary number of following hexadecimal digits, so an unescaped hex-looking byte immediately after it could change the generated value. The accepted implementation escapes quote and backslash, uses the standard single-character C escapes where appropriate, and emits other bytes as fixed-width three-digit octal escapes, whose lexical extent is bounded. Release-4.2 MR !15132 carries the same fix.

**Implementation rule:** when exporting C, JSON, shell text, display filters, or another formal syntax, choose an escaping/serialization primitive whose token boundary is unambiguous for arbitrary following input. Include the language's delimiter and escape characters themselves in the audit; testing only non-printable bytes is insufficient.

**Submission rule:** a small correctness fix needed on a maintained branch need not be blocked on introducing a broader public helper API. Land the narrowly scoped grammar-correct fix when appropriate, then consider reusable API consolidation separately.

**Confidence:** Very high. The master fix was authored and merged by John Thacker, received positive maintainer review, and was subsequently carried to the maintained 4.2 branch.
