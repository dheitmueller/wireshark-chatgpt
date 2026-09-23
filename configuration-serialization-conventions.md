# Wireshark Configuration Serialization Conventions

This file records durable rules for serializing user-controlled values into Wireshark configuration and state files. Current upstream source remains authoritative.

## Sanitize record delimiters before writing values to line-oriented files

When a file format uses line boundaries as record or directive boundaries, an embedded newline in a user-controlled value is not ordinary payload data: it can create an additional logical record when the file is read back.

Merged master MR !23628, authored by John Thacker, sanitizes newline characters in user-provided strings before writing preferences, recent-state, and filter files that are parsed a line at a time. The change was accepted upstream as a correctness/security hardening measure across multiple persistence paths.

**Implementation rule:** before serializing user-controlled text into a line-oriented configuration/state format, remove or encode the format's record separators according to that format's escaping contract. Do not rely on the reader to infer that an injected newline belonged to the preceding value.

**Review rule:** when adding a persisted string field, review both its writer and reader together. Identify the syntax delimiters that create records, fields, comments, or directives and ensure untrusted/user-entered values cannot synthesize those delimiters unless the format defines an unambiguous escaping mechanism.

**Compatibility note:** serialization hardening must still respect Wireshark's supported dependency baseline. A post-merge !23628 report identified use of a GLib constant newer than Ubuntu 22.04's supported GLib, illustrating that a sound semantic fix can still need portability follow-up if implemented with too-new library API.

**Confidence:** Very high for the delimiter rule. The master change was authored by John Thacker and merged upstream; the compatibility note is supporting review evidence rather than the basis of the rule.

## Preserve legacy spellings while allowing an extensible numeric domain

Persistent state should not require a parser/table edit every time an enum-like numeric domain grows, but changing the serialized spelling can break older Wireshark versions that read the same profile or `recent` file. When both goals matter, readers can accept both legacy symbolic names and the numeric representation, while writers continue to emit the old symbolic spelling for values that already had one and use numbers only for newer values.

Merged master MR !12133, authored by Guy Harris, applies that design to timestamp display precision. Older `recent` files used names such as `AUTO`, `SEC`, `USEC`, and `NSEC`; the underlying setting is now treated as either one of those legacy names or a numeric precision from 0 through `WS_TSPREC_MAX`. The writer retains names where one exists so older Wireshark releases can still understand established values, while numeric serialization lets the supported precision set expand without growing an artificial name table.

A Coverity finding immediately exposed an important implementation detail in that change. The first range check accidentally compared a nonnegative numeric precision against `TS_PREC_AUTO`, a negative sentinel. Merged follow-up !12157, also authored by Guy Harris, corrects the upper bound to `WS_TSPREC_MAX`.

**Compatibility rule:** when evolving an enum-like persisted value, separate backward-compatible spellings from the value domain itself. Accept historical names, preserve them when writing established values if older readers depend on them, and use a stable numeric form for values whose meaning is inherently numeric and expected to grow.

**Implementation rule:** do not use a special sentinel as though it were a numeric-domain bound. Keep sentinel values such as `AUTO` distinct from the minimum/maximum of the ordinary value range, and name/check the range against the actual domain constant.

**Testing rule:** cover at least one legacy symbolic value, the minimum and maximum numeric values, an out-of-range number, and the sentinel/fallback path. Configuration compatibility deserves reader-and-writer tests because a file can be accepted by the current build while still being needlessly unreadable by an older supported release.

**Confidence:** Extremely high. The serialization design and the corrective follow-up were both merged master changes authored by Guy Harris; the boundary mistake was independently detected by Coverity and fixed immediately.