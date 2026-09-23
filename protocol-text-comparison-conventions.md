# Wireshark Protocol Text Comparison Conventions

This file records durable conventions for comparing textual protocol tokens and other machine-defined text. Current upstream APIs and protocol specifications remain authoritative.

## Keep ASCII protocol comparisons independent of the process locale

When a protocol, capture format, or dissector lookup defines tokens in an ASCII-like machine syntax, case-insensitive comparison must use locale-independent ASCII semantics. Do not rely on the host C library's locale-sensitive case folding merely because Wireshark normally runs in a UTF-8 locale. A user locale can change the meaning of case conversion; the Turkic `i`/`I` mappings are a concrete example where locale-sensitive `strcasestr()` can behave differently from an ASCII protocol comparison.

Merged master MR !12552, authored and merged by John Thacker, replaced use of the system `strcasestr()` with a locale-independent implementation for Wireshark's protocol-oriented string search. Guy Harris specifically suggested naming the helper `ws_ascii_strcasestr()` so its semantic domain is explicit rather than hidden behind the generic-looking `ws_strcasestr()` name. John separated the behavior fix from the API rename because the existing helper predated 4.0: the semantic correction could be cherry-picked safely to stable branches first, while the rename could be handled as a distinct compatibility-sensitive change. That family landed as !12552 plus master rename !12555 and stable behavior/rename backports !12556, !12557, and !12558.

**Implementation rule:** choose text comparison/folding behavior from the syntax being parsed, not from the user's locale. For ASCII-defined wire/control tokens, prefer explicitly ASCII comparison helpers whose names make that contract visible.

**API rule:** helper names should expose important comparison domains such as ASCII versus locale-aware text. A generic name that hides a material semantic constraint invites incorrect reuse.

**Backport rule:** when a correctness fix and an API rename are coupled conceptually but have different compatibility risk, split them so the behavioral fix can be backported without unnecessarily carrying the source/API churn.

**Confidence:** Very high. The master implementation merged, Guy Harris gave direct naming/semantic feedback, and both maintained release branches received the behavioral fix; the rename was intentionally handled separately for compatibility reasons.
