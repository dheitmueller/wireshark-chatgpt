# Wireshark String Field Termination Conventions

This file records durable conventions for choosing `FT_STRING`, `FT_STRINGZ`, `FT_STRINGZPAD`, and `FT_STRINGZTRUNC` from the wire contract rather than from how sample data happens to look.

## Distinguish null termination from null padding and null truncation

Merged master MR !234, authored by Guy Harris, introduces `FT_STRINGZTRUNC` to represent a fixed-width string field whose meaningful string may end at a NUL, while bytes after that NUL are not guaranteed to be zero. This is semantically different from `FT_STRINGZPAD`, where the remaining bytes are defined as NUL padding.

The surrounding fixes make the taxonomy concrete:

- !224: AFP passwords occupy eight octets and shorter passwords are actually padded with NULs, so `FT_STRINGZPAD` is appropriate.
- !212 (master) and stable !221: the GenDC signature is exactly four ASCII characters and explicitly not NUL-terminated, so it is `FT_STRING`.
- !223: the Aeron Error String is not specified or observed as NUL-terminated, so it is `FT_STRING`, not `FT_STRINGZ`.
- !213: Guy records an NCP length-delimited value where a NUL can be followed by nonzero bytes, foreshadowing the need for null-truncated semantics.
- !225: an interim stable SAP change uses `FT_STRINGZPAD`, but the quoted protocol text says bytes after a terminator are undefined; !234's later master change to `FT_STRINGZTRUNC` is the stronger final precedent.

**Field-definition rule:** select the string field type from the protocol's termination and padding guarantees, not from one capture. A terminator does not imply padding, and a fixed-width textual area does not imply a terminator.

**Review rule:** when changing a string field type, inspect the protocol wording for all three questions separately: Is termination required? Can the string occupy the full fixed width without a terminator? What, if anything, is guaranteed about bytes after an early terminator?

## Use string-family predicates when behavior truly applies to all string types

!234 also replaces explicit enumerations of individual string field types with `IS_FT_STRING()` where the operation applies to the whole string family.

**Maintenance rule:** when code is intended to apply uniformly to all Wireshark string types, prefer the common type predicate over a hand-maintained switch/list. Enumerate individual types only when their semantics actually differ.

**Confidence:** Extremely high. The core type is introduced in merged master work authored by Guy Harris and is supported by several adjacent merged protocol corrections.
