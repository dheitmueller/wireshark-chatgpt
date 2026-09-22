# Wireshark Binary-Data Representation Conventions

This file records durable conventions for converting textual configuration/input into opaque binary values and carrying those values through APIs. Current upstream source remains authoritative.

## Normalize textual encoding once, then keep opaque data as bytes plus an explicit length

When user-facing syntax is only an encoded representation of opaque bytes, perform decoding and syntax normalization at the parsing boundary and carry the result in a binary representation with an explicit length. Do not repeatedly convert between strings and byte arrays downstream, and do not rely on NUL termination when the underlying value can legally contain zero bytes.

Merged master MR !13219, authored and merged by John Thacker, moves WPA passphrase percent-decoding into `parse_key_string()` so constraints such as passphrase length are checked against the decoded value rather than its textual `%xx` representation. John explicitly notes that preserving percent-encoded embedded NUL bytes requires API changes so the decoded value is not later treated as an ordinary C string.

Merged master MR !13233 completes that representation change: parsed 802.11 keys are stored as `GByteArray`, redundant string/byte conversions and copies are removed, and the downstream dissector relies on the parser's typed/validated result rather than repeating the same validation. The explicit byte length also permits opaque WPA password data containing internal NULs.

**Parsing rule:** normalize escapes, percent encoding, and similar textual transport syntax before applying constraints whose semantics are defined on the decoded value. Reject ambiguous delimiter syntax before decoding when decoding can introduce the delimiter and make the user's intended split unknowable.

**Representation rule:** after the parsing boundary, use a byte container plus explicit length for opaque binary values. Preserve string representations only where textual identity is itself semantically required. Downstream consumers should consume the validated typed result instead of re-parsing or re-validating the original textual syntax.

## Audit units and derived metadata when changing representation

A representation refactor can leave derived quantities with formulas appropriate to the old representation. The compiler may not catch those errors when the types remain numerically compatible.

Merged !13247, also authored and merged by John Thacker, fixes the immediate follow-up from !13233: once a key's `len` is a count of bytes rather than a count associated with hexadecimal string characters, its bit length is `len * 8`, not `len * 4`. The field happened not to be used at that point, but the stale unit conversion was corrected rather than left as latent wrong state.

**Review rule:** when changing from text to bytes, encoded length to decoded length, words to bytes, or another representation with different units, audit every cached/derived length, bit count, offset, comparison, serialization path, and display value that consumes the representation. Do not assume an unused derived field is harmless; stale semantic state tends to become future behavior.

**Confidence:** Very high. Three adjacent merged master changes authored and merged by John Thacker establish the parsing boundary, binary representation, downstream-validation ownership, and unit-audit consequences.