# Wireshark Search-Offset Conventions

This file records durable conventions for byte and regular-expression search state. Current upstream source and the underlying search-library contracts remain authoritative.

## Represent match position as start plus length

Search state should use a start offset plus a length rather than storing an inclusive "last byte" position. Start-plus-length matches the usual buffer-range model, avoids repeated `+1`/`-1` arithmetic, and can represent a zero-length match without inventing a byte that belongs to the match.

Merged master MR !12416, authored and merged by John Thacker, changes packet Find state so `search_pos` is the first byte of the match rather than the last byte. The previous representation required confusing endpoint arithmetic and could not correctly model zero-length regular-expression matches. The accepted code treats the match length independently for highlighting and progression.

**Implementation rule:** model a match or byte range as `[start, start + length)`. Do not encode match existence or range ownership by overloading an inclusive endpoint. If zero-length matches are possible, define their progression and highlighting behavior explicitly so repeated search cannot stall or manufacture a byte of extent.

## Preserve the complete regular-expression subject when resuming a search

A regular-expression engine's start-offset parameter is semantically different from passing a pointer to a shortened suffix of the subject. Assertions such as lookbehind, beginning-of-line/subject tests, and related context can inspect data before the next candidate match position. Slicing the subject destroys that context and can change the result.

Merged master MR !12418, also authored and merged by John Thacker, extends `ws_regex_matches_pos()` with a `subj_offset` and passes it to PCRE2 as `startoffset` while preserving the original subject and length. The MR explicitly documents PCRE2's warning that a nonzero start offset is not equivalent to shortening the subject and setting `PCRE2_NOTBOL`, particularly for patterns beginning with lookbehind.

**Implementation rule:** when continuing a regex search within packet bytes or another larger logical subject, keep the original subject base and full semantic length and advance using the regex engine's supported start-offset mechanism. Do not advance by rebasing the subject pointer unless the intended semantics truly are a new independent subject.

**Review rule:** test resumed searches with context-sensitive expressions, especially lookbehind and zero-length matches, rather than only ordinary consuming patterns. An implementation that works for simple literals may still have changed the regex subject's semantics.

**Confidence:** Extremely high. Both !12416 and !12418 are merged master changes authored, approved, and merged by John Thacker, and the latter directly preserves the underlying PCRE2 API contract.