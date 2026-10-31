
# Wireshark Text Encoding Conventions

This file records reusable conventions for converting protocol text encodings into Wireshark's common string representation.

## Put generally reusable character sets in the common encoding layer

When a protocol uses a standardized character set that is useful beyond one dissector, prefer implementing it once in Wireshark's common encoding/tvbuff machinery rather than embedding a private translation table in the protocol dissector. The central implementation should expose the encoding through the normal `ENC_*` path and keep the associated IANA-character-set mapping, introspection, documentation, and string-extraction APIs coherent.

Merged master MR !10427, authored and merged by John Thacker, adds EBCDIC code page 500 for DRDA. Instead of teaching only DRDA how to translate CP500, the change adds the code-page table to `epan/charsets`, adds `ENC_EBCDIC_CP500`, teaches the tvbuff string APIs to decode it, marks IANA IBM500 as supported, exposes the enum through introspection, documents it in `README.dissector`, and then has DRDA select that shared encoding.

**Architecture rule:** protocol dissectors should identify the on-wire encoding; common character-set conversion belongs in the shared text-decoding layer when the encoding has general meaning. This avoids parallel conversion code and lets other dissectors, Lua/introspection users, and generic string APIs share the same mapping.

**Confidence:** Very high. Merged master implementation authored and merged by John Thacker.

## Truncate rendered text on UTF-8 boundaries and preserve the full underlying value

A byte-counted string formatting precision can cut through a UTF-8 multibyte sequence and produce invalid output. Character counts and rendered display-cell widths are not interchangeable either. Display abbreviation must not accidentally shorten metadata that should be retained for later protocol processing.

Merged John Thacker MR !9772 changes DICOM tag-value display from `%-50.50s...` to `ws_utf8_truncate(vals, 50)` plus `UTF8_HORIZONTAL_ELLIPSIS`, preventing a split character. It copies a protocol error-comment value into file-scoped storage **before** deriving the shortened presentation form, so later data consumers retain the original information. The MR explicitly notes that 50 bytes, 50 code points, and a display width of 50 are not synonymous.

**Review rule:** identify whether a truncation limit means bytes, Unicode code points, grapheme clusters, or visual width; use an appropriate helper and test multibyte characters exactly at the boundary. Keep the complete protocol value in its correct allocator scope and generate an abbreviated view for the UI independently.

**Confidence:** Very high. Merged mainline correctness fix authored and merged by John Thacker with explicit UTF-8 semantics.
