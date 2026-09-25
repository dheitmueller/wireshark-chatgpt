
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

## Preserve format-specific string semantics when sanitizing UTF-8

A generic library routine can be Unicode-correct yet still be wrong for a file or protocol format if that format assigns special meaning to embedded NULs or malformed sequences. Prefer Wireshark's shared text helpers when they encode the project's required semantics, and centralize those semantics instead of open-coding slightly different conversions in each consumer.

Merged !9705 fixes pcapng option handling because GLib's `g_utf8_make_valid()` replaces embedded NUL bytes with replacement characters, while pcapng string options are terminated by the first NUL. The accepted change moves the existing Wireshark UTF-8 repair implementation into `wsutil/unicode-utils` and uses it from wiretap. John Thacker verified the fix against a real Apple-generated pcapng sample. Merged !9711 then converts additional users to the shared helper.

**Implementation rule:** choose a string-validation/sanitization helper based on both Unicode validity and the enclosing format's byte/string contract. If multiple subsystems need the same non-default behavior, put it in a shared Wireshark utility rather than duplicating it.

**Review/tooling rule:** do not automatically ban a broadly valid upstream API merely because Wireshark has a preferred helper for some cases. In !9711 Gerald Combs asked whether `g_utf8_make_valid` should be added to `checkAPIs.pl`; João Valverde preferred, at most, a gentle hint rather than a ban because the API is not universally wrong.

**Confidence:** Very high. Both changes were merged; !9705 has direct real-capture validation and !9711 includes explicit maintainer discussion about enforcement strength.

## Use encoding-aware TVBuff extraction even for nominally ASCII strings

A protocol string being specified as ASCII does not make a raw byte copy equivalent to string decoding. Captured traffic can contain invalid bytes, and callers that surface the result in columns or other text interfaces should receive Wireshark's normal replacement/validation behavior rather than an unchecked byte buffer.

Merged master MR !9489, authored and merged by John Thacker, fixes EtherCAT FoE filename formatting by replacing a fixed local buffer plus `tvb_memcpy()` with `tvb_get_string_enc(..., ENC_ASCII)`. The MR rationale explicitly notes that even an expected-ASCII field can contain errors; using the encoding-aware TVBuff helper makes malformed input safe and gives it the same text-conversion semantics as other decoded strings.

**Implementation rule:** when bytes are semantically text, materialize them with the appropriate `tvb_get_string*_enc()`/tree string API instead of copying raw bytes into a C string. Reserve raw `tvb_memcpy()` for data that is actually byte-oriented.

**Confidence:** Extremely high. Merged master correction authored and merged by John Thacker, with the malformed-encoding rationale stated directly in the MR.

## Advance decoder state by consumed input, not merely by produced output

Malformed-text recovery has two independent quantities: bytes consumed from the source and bytes copied into valid output. A decoder must advance its cursor and remaining-length accounting by the former even when an invalid sequence produces no directly copied bytes. Conflating them can duplicate replacement characters, reprocess input, or mishandle adjacent invalid bytes.

Merged master MR !9214, authored and merged by Gerald Combs with corrective review from John Thacker, fixes UTF-8 recovery so the remaining length is reduced by ptr - prev, the complete source span consumed by validation, rather than only by the number of valid bytes. John specifically noted that the wrong accounting could append extra replacement characters. Merged master MR !9248, authored and merged by John Thacker, fixes the analogous ASCII conversion state after !9224's batching optimization: prev must advance past an invalid byte even when two invalid bytes are adjacent and there were zero valid bytes to append between them.

**Implementation rule:** keep source progress, valid-output length, and replacement emission as separate state. After a validation step, update the source cursor/remaining length for every consumed byte regardless of whether the output path appended a valid run.

**Testing rule:** text-decoder optimizations must include adjacent invalid bytes, malformed/truncated multibyte sequences, invalid bytes at run boundaries, and valid text on both sides. These cases expose progress bugs that a mostly-valid sample will not.

**Confidence:** Extremely high. Two merged core-charset correctness fixes, with the UTF-8 accounting correction supplied directly by John Thacker.

## Document wire-order transformations that look opposite to the access API

Sometimes a specification defines text as byte groups inside endian-sensitive machine words, making the correct implementation look backwards when viewed only at the integer-read call. In those cases, document the complete transformation from wire bytes to text rather than changing one apparently inverted endian operation in isolation.

Merged master MR !9226, authored by Guy Harris, expands the SRT string-decoding comments to explain the protocol's unusual representation: the payload is stored as 32-bit little-endian words, while the string octets within each word are consumed in the corresponding reversed byte order, and unused bytes are NUL padding rather than a NUL terminator. The implementation's big-endian word read followed by low-to-high byte extraction is therefore equivalent to a little-endian word read followed by high-to-low extraction.

**Implementation rule:** when byte order, bit extraction, or padding rules make the code intentionally non-obvious, place a specification-level equivalence explanation next to the transform. Distinguish padding from termination because the latter changes string length and malformed-input behavior.

**Confidence:** Extremely high. Merged explanatory change authored by Guy Harris for an otherwise counterintuitive on-wire text representation.
