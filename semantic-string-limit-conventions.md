# Semantic String Limit Conventions

Merged MRs !8933 and !8946 remove label-sized limits from generic string construction: the protocol value remains complete, while the UI owns safe presentation truncation. Merged MR !8919 independently keeps the full SIP Call-ID while allowing a separate bounded representation for lookup.

**Rule:** a UI label size or lookup-key limit must not silently redefine the protocol value. Preserve the complete semantic string unless the protocol itself defines a limit.

**Lifetime:** when allocator scope already owns a string buffer, a borrowed string pointer can avoid unnecessary finalization and reallocation.

**Confidence:** Very high; three merged master changes.
