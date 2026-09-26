
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


## Do not truncate composed UTF-8 with arbitrary fixed byte buffers

A display string assembled from decoded text and formatted metadata should not be given an arbitrary C-buffer size unless the protocol itself imposes that byte limit. Byte-count truncation can split a multibyte UTF-8 sequence and turn otherwise valid text into invalid output.

Merged master MR !9132, authored and merged by João Valverde, fixes C15 display strings by replacing fixed-size buffers and repeated bounded concatenation with dynamically sized Wireshark string-building APIs. The old limits were presentation implementation details rather than protocol limits and could truncate a UTF-8 string in the middle of a code point.

**Implementation rule:** use dynamically sized string-building APIs for human-readable composed text. If the UI or protocol truly requires truncation, apply the semantic limit on a valid character boundary rather than by cutting an arbitrary number of UTF-8 bytes.

**Review rule:** distinguish a protocol maximum length from a temporary-buffer capacity. A magic display-buffer size should not silently become a protocol or presentation limit.

**Confidence:** Very high. Merged master text-correctness fix authored and merged by João Valverde.

## Keep source-byte progress separate from rendered diagnostic width

Merged master MR !9108, authored by João Valverde, rewrites UTF-8 logging diagnostics around separate source-length, display, and underline state. A malformed source byte can expand into a multi-character hexadecimal escape, so the diagnostic marker width must follow the rendered form while source traversal advances by the bytes consumed.

**Implementation rule:** keep input consumption and rendered-column production as separate quantities when displaying repaired or escaped text. Size carets and underlines from the rendered text, while parser progress remains tied to source bytes.

Merged master MR !9076, authored by John Thacker, independently replaces a raw four-byte USB Video FourCC copy plus manual NUL termination with `tvb_format_text()`. Merged !9100 applies the same text-safety principle across several dissectors, including `tvb_get_string_enc(..., ENC_ASCII)` for NCSI firmware text. These changes reinforce the existing rule that packet bytes used as text should pass through Wireshark's text-aware extraction and formatting helpers.

**Confidence:** Very high. Multiple merged master fixes, including one authored by John Thacker.

## Document ambiguous deployed encodings and make the discriminator explicit

When two applicable specifications or deployed ecosystems assign different text encodings to the same field, do not hide the ambiguity behind a comment that labels one branch “correct” and the other “broken.” State the competing contracts and make the heuristic used to distinguish them explicit, conservative, and reviewable.

Merged master MR !8884 handles the Mobile IPv6 Service Selection Mobility Option. Review by João Valverde corrected the initial interpretation: RFC 5149 permits a UTF-8 identifier, while deployed 3GPP behavior encodes APN-style labels. John Thacker acknowledged the standards conflict and the final code documents both sources, using the leading octet as a heuristic to select `ENC_APN_STR` versus `ENC_UTF_8`.

**Implementation rule:** when compatibility requires heuristic decoding, keep standards provenance in the code and separate “what the specification says” from “what captures in the field do.” Prefer a discriminator that fails conservatively and leaves unusual input inspectable rather than silently normalizing all traffic into one assumed encoding.

**Confidence:** High. Merged master interoperability fix with substantive review correcting the standards interpretation before acceptance.
## Keep wire-byte geometry separate from decoded-text geometry

Transformations defined on the encoded octets belong in the byte domain, before character decoding. Conversely, once bytes have been decoded or sanitized into UTF-8, the original wire-byte count is no longer a safe character or output-byte bound because replacement and escaping can change the representation length.

Merged master MR !8623, authored and merged by John Thacker, fixes World of Warcraft fields that store four ASCII octets in reverse order. The old code decoded to UTF-8 first and then called `g_strreverse()`; malformed ASCII can decode to the multibyte replacement character, so byte-wise reversal at that point corrupts UTF-8. The accepted code copies/reverses the four raw bytes first and only then converts them as ASCII.

Merged master MR !8638, also authored by John Thacker, fixes SCTP's counted-and-NUL-terminated Host Name parameter. It registers the field as `FT_STRINGZ`, obtains the normal display string from the protocol-tree API, and stops passing the wire octet length as a `%.*s` width. John explicitly notes that replacement or escaping means the UTF-8 display representation need not have the same octet length as the packet field. Merged !8658 carries the resulting host-name handling to a maintained branch. Merged master !8634 independently reinforces the same boundary by using `ENC_APN_STR` for DNS-label-style GTP FQDNs rather than manually mutating bytes into a dotted string.

**Implementation rule:** perform protocol-defined byte reordering, packing, or delimiter interpretation before decoding to Unicode. After decoding, treat the returned UTF-8 string according to the string API's own length/termination contract rather than reusing a source-wire byte count as a presentation bound.

**Review rule:** whenever one variable is used both as a TVBuff byte length and as a width/index into decoded text, check whether conversion can replace, escape, normalize, or expand input. If it can, those are different coordinate spaces and need different quantities.

**Confidence:** Extremely high. Multiple merged master text-decoding fixes, with the two central examples authored and merged by John Thacker.
