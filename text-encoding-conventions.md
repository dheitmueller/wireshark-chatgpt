
# Wireshark Text Encoding Conventions

This file records reusable conventions for converting protocol text encodings into Wireshark's common string representation.

## Put generally reusable character sets in the common encoding layer

When a protocol uses a standardized character set that is useful beyond one dissector, prefer implementing it once in Wireshark's common encoding/tvbuff machinery rather than embedding a private translation table in the protocol dissector. The central implementation should expose the encoding through the normal `ENC_*` path and keep the associated IANA-character-set mapping, introspection, documentation, and string-extraction APIs coherent.

Merged master MR !10427, authored and merged by John Thacker, adds EBCDIC code page 500 for DRDA. Instead of teaching only DRDA how to translate CP500, the change adds the code-page table to `epan/charsets`, adds `ENC_EBCDIC_CP500`, teaches the tvbuff string APIs to decode it, marks IANA IBM500 as supported, exposes the enum through introspection, documents it in `README.dissector`, and then has DRDA select that shared encoding.

**Architecture rule:** protocol dissectors should identify the on-wire encoding; common character-set conversion belongs in the shared text-decoding layer when the encoding has general meaning. This avoids parallel conversion code and lets other dissectors, Lua/introspection users, and generic string APIs share the same mapping.

**Confidence:** Very high. Merged master implementation authored and merged by John Thacker.

## Prefer TVBuff string-item APIs over manual copy-and-terminate code

When a protocol field is already represented by a registered string field and the bytes are contiguous in a TVBuff, prefer adding the item with the correct encoding instead of allocating a temporary buffer, copying bytes, appending a NUL, and then calling a string-specific tree API. The TVBuff/proto-tree path centralizes bounds, encoding, and string-validation behavior.

Merged master MR !9720, authored and merged by John Thacker, replaces several PROFINET RSI sequences of `wmem_alloc()` + `tvb_memcpy()` + manual NUL termination + `proto_tree_add_string()` with direct `proto_tree_add_item(..., ENC_UTF_8)` calls. The MR rationale explicitly notes that this lets the normal API validate encoding and trailing NULs while eliminating the temporary allocations/copies.

**Implementation rule:** when the registered `hf_` type and on-wire encoding already describe the string, add it directly from the TVBuff with the appropriate `ENC_*` value. Use explicit extraction/copying only when the caller truly needs a separately materialized or transformed string.

**Confidence:** Extremely high. Merged cleanup authored and merged by John Thacker, with the validation and allocation benefits stated directly in the MR.
