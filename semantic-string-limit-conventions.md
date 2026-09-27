# Semantic String Limit Conventions

Merged MRs !8933 and !8946 remove label-sized limits from generic string construction: the protocol value remains complete, while the UI owns safe presentation truncation. Merged MR !8919 independently keeps the full SIP Call-ID while allowing a separate bounded representation for lookup.

**Rule:** a UI label size or lookup-key limit must not silently redefine the protocol value. Preserve the complete semantic string unless the protocol itself defines a limit.

**Lifetime:** when allocator scope already owns a string buffer, a borrowed string pointer can avoid unnecessary finalization and reallocation.

**Confidence:** Very high; three merged master changes.


## Configuration expression length is independent of displayed column width

Merged MR !7358 removes `COL_MAX_LEN` from custom-column expression parsing. John Thacker notes that a custom-column expression can legitimately contain many semantically equivalent fields even though only one value appears in any given frame, so expression length has no necessary relationship to displayed column width. The accepted parser uses `GString` instead of a fixed presentation-sized temporary buffer.

**Rule:** do not reuse a UI label/column limit as a parser or persistence limit for semantic configuration data unless the configuration format itself defines that bound. Use growable storage when the semantic object is not intrinsically fixed-size.

**Confidence:** Very high. Merged master correctness fix authored by John Thacker.
