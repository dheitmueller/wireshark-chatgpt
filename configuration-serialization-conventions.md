# Wireshark Configuration Serialization Conventions

This file records durable rules for serializing user-controlled values into Wireshark configuration and state files. Current upstream source remains authoritative.

## Sanitize record delimiters before writing values to line-oriented files

When a file format uses line boundaries as record or directive boundaries, an embedded newline in a user-controlled value is not ordinary payload data: it can create an additional logical record when the file is read back.

Merged master MR !23628, authored by John Thacker, sanitizes newline characters in user-provided strings before writing preferences, recent-state, and filter files that are parsed a line at a time. The change was accepted upstream as a correctness/security hardening measure across multiple persistence paths.

**Implementation rule:** before serializing user-controlled text into a line-oriented configuration/state format, remove or encode the format's record separators according to that format's escaping contract. Do not rely on the reader to infer that an injected newline belonged to the preceding value.

**Review rule:** when adding a persisted string field, review both its writer and reader together. Identify the syntax delimiters that create records, fields, comments, or directives and ensure untrusted/user-entered values cannot synthesize those delimiters unless the format defines an unambiguous escaping mechanism.

**Compatibility note:** serialization hardening must still respect Wireshark's supported dependency baseline. A post-merge !23628 report identified use of a GLib constant newer than Ubuntu 22.04's supported GLib, illustrating that a sound semantic fix can still need portability follow-up if implemented with too-new library API.

**Confidence:** Very high for the delimiter rule. The master change was authored by John Thacker and merged upstream; the compatibility note is supporting review evidence rather than the basis of the rule.
