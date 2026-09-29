# Wireshark Format-Truncation Analysis Conventions

Merged master MR !5460, authored by João Valverde, exposed GCC format-truncation diagnostics while converting ASN.1-related formatting to standard stdio. Guy Harris stated that genuinely truncatable output should use storage large enough for the complete string, while noting that allocator/lifetime choices matter; João also noted that some diagnostics are conservative when protocol bounds make truncation impossible.

**Rule:** investigate each truncation warning against the real input/output contract. If truncation can lose meaningful output, fix sizing or representation instead of globally suppressing the diagnostic. When replacing formatting APIs, re-check capacity, terminator, return-value, and lifetime semantics.

**Confidence:** Extremely high for the design direction because it comes directly from Guy Harris in a merged master change.
