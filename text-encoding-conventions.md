
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

## Decode tvbuff-backed protocol strings directly with a declared encoding

Merged master !9720, authored and merged by John Thacker, replaces six PROFINET string paths built from `wmem_alloc`, `tvb_memcpy`, manual NUL placement and `proto_tree_add_string` with `proto_tree_add_item(..., ENC_UTF_8)`. This enables central encoding and trailing-NUL validation without maintaining temporary character buffers.

**Rule:** When the bytes are already in a tvbuff, prefer an encoding-aware field API; maintain separate explicit cursor increments for protocol blank/padding bytes. **Confidence:** Very high; merged John Thacker patch.

## Decode arbitrary-length UTF-8 using a shared validator with explicit allocation scope

Merged master !9705 moves UTF-8 sanitization into `wsutil/unicode-utils`, where `ws_utf8_make_valid(scope, bytes, length)` can handle bounded capture-option data and Unicode replacement of ill-formed sequences without treating embedded NUL bytes as malformed UTF-8. John Thacker validated the pcapng change with a real Apple-produced capture containing NULs at the ends of string options. Merged !9711 extends this central API to a `wmem_strbuf` form and uses packet-scoped storage when sanitizing HTTP/2 headers for display.

**Implementation rule:** validate exactly the specified byte span, use Wireshark's shared UTF-8 replacement semantics, and allocate the sanitized result in the scope appropriate to its downstream consumer. A NUL is valid UTF-8 but may still be prohibited or specially handled by the particular destination string API; keep those two contracts separate. Preserve the distinction between raw captured bytes, cleaned Unicode text, and ownership of the destination field/block.

**Review nuance:** Gerald Combs proposed banning direct `g_utf8_make_valid` calls via `checkAPIs.pl`; João Valverde preferred a gentle checker hint over a hard ban. Treat the merged replacement helper as accepted behavior but the proposed outright ban as *not* settled policy. **Confidence:** very high for implementation and sampled encoding behavior; the tool-enforcement policy remained debated.
