
# Wireshark Text Encoding Conventions

This file records reusable conventions for converting protocol text encodings into Wireshark's common string representation.

## Put generally reusable character sets in the common encoding layer

When a protocol uses a standardized character set that is useful beyond one dissector, prefer implementing it once in Wireshark's common encoding/tvbuff machinery rather than embedding a private translation table in the protocol dissector. The central implementation should expose the encoding through the normal `ENC_*` path and keep the associated IANA-character-set mapping, introspection, documentation, and string-extraction APIs coherent.

Merged master MR !10427, authored and merged by John Thacker, adds EBCDIC code page 500 for DRDA. Instead of teaching only DRDA how to translate CP500, the change adds the code-page table to `epan/charsets`, adds `ENC_EBCDIC_CP500`, teaches the tvbuff string APIs to decode it, marks IANA IBM500 as supported, exposes the enum through introspection, documents it in `README.dissector`, and then has DRDA select that shared encoding.

**Architecture rule:** protocol dissectors should identify the on-wire encoding; common character-set conversion belongs in the shared text-decoding layer when the encoding has general meaning. This avoids parallel conversion code and lets other dissectors, Lua/introspection users, and generic string APIs share the same mapping.

**Confidence:** Very high. Merged master implementation authored and merged by John Thacker.


## Treat encoding values as composable flags and carry protocol byte order explicitly

Wireshark text encodings can combine a character-set selector with orthogonal properties such as byte order. When a protocol configuration determines endianness, pass that choice into the shared string-decoding API instead of assuming the character-set token alone is sufficient. Code that later asks which character set is active must likewise test the relevant flag bit rather than comparing the complete combined value for equality.

Merged master MR !1709 fixes SOME/IP UTF-16 strings that were decoded with the wrong byte order when no BOM was present. The accepted code uses `ENC_UTF_16 | ENC_BIG_ENDIAN` or `ENC_UTF_16 | ENC_LITTLE_ENDIAN` according to the configured protocol setting, uses `ENC_NA` with UTF-8/ASCII, and changes later ASCII/UTF-8 tests from exact equality to flag membership because the encoding value now carries multiple pieces of information. The same fix adjusts the protocol-tree item's end after the variable-length string is consumed so the highlighted source range matches the parsed value.

**Implementation rule:** treat `ENC_*` values according to their API semantics: compose the character encoding with required byte-order/other flags, and test component flags when the value may contain more than one bit. Do not infer UTF-16 byte order from host order or from a missing BOM when the protocol already specifies it.

**Presentation rule:** when an item's true extent is known only after parsing variable-length content, update the item end so tree highlighting reflects the bytes actually consumed.

**Confidence:** High. Merged master correctness fix, approved/merged by Anders Broman.
