# Wireshark Custom Column Conventions

This file records durable conventions for custom-column construction and related model presentation. Current upstream implementation remains authoritative.

## Keep resolved display text separate from unresolved/filter representation

Custom columns can need both a human-readable, resolved representation and an unresolved/value-oriented representation. Those strings have different semantics and can have different lengths. Do not build one by splicing or truncating portions of the other.

Merged master MR !8826, authored by John Thacker, fixes several boundary failures near the custom-column size limit. The accepted code constructs the resolved `result` and unresolved `expr` independently; the old path could truncate the resolved string and later reuse offsets/substrings while constructing the expression, producing malformed output for mixed field types. The same change fixes separator insertion near the limit and avoids passing a NULL mapping result to formatting code.

**Implementation rule:** derive display text and expression/value text independently from the field value and metadata. A truncation, name resolution, or formatting decision in one representation must not alter the semantic content or offsets of the other.

**Confidence:** Very high. Merged master correctness fix authored by John Thacker.

## Truncation is a presentation policy and must preserve UTF-8 boundaries

When a bounded custom-column display is truncated, make that fact visible and truncate with a UTF-8-aware helper. Do not silently cut raw bytes or allow a presentation limit to change the value used to construct filters.

MR !8826 explicitly notes that custom-column display text is no longer used to construct the filter expression. It therefore marks a truncated display value as truncated and uses the normal label truncation path so the boundary cannot split a multibyte UTF-8 character.

**Implementation rule:** apply size limits to the presentation representation, mark truncation where practical, and keep filter/value semantics independent of the bounded display string.

**Confidence:** Very high. Same merged master fix, with the display-only role and UTF-8-safe truncation stated in the change rationale.
