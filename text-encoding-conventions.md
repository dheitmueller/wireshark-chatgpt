
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

## Decode captured text with the protocol's declared encoding before placing it in columns

Merged master !9489 (John Thacker) fixes EtherCAT FoE filename/error strings displayed in columns. The old `tvb_memcpy` into a raw char array treated wire bytes as already suitable for display; the accepted change calls `tvb_get_string_enc(wmem_packet_scope(), ..., ENC_ASCII)`, bounds the source length, and uses an empty fallback when no string was present.

**Rule:** even purported ASCII protocol bytes can contain invalid sequences. Decode or sanitize with Wireshark's encoding-aware TVB string helpers rather than copying into a NUL-terminated presentation buffer; provide a safe output for absent payloads and match the allocation lifetime to the column consumer. **Confidence:** high, merged John Thacker bug fix for actual column output.

## Count the entire consumed invalid sequence when repairing UTF-8

Merged master !9214 (Gerald Combs) fixes an invalid UTF-8 decoder that could read or emit past the valid input length. John Thacker's direct review corrected the essential accounting: decrement remaining bytes by the **full validator-consumed span** `ptr - prev`, not just the number of valid prefix bytes; then advance past the valid segment before deciding whether to append U+FFFD. The accepted implementation avoids spurious replacement characters at the end and preserves the Unicode maximal-subpart replacement intent.

A related pair of John Thacker fixes, merged !9224 and !9248, batches contiguous valid ASCII bytes for efficient appending but also advances `prev` correctly when **two invalid ASCII bytes occur consecutively**. Merely optimizing the common valid run is insufficient if pointer state drifts on malformed runs. Merged !9217 validates and repairs SRT's protocol-specific UTF-8 text before presenting it, while documenting the unusual storage as 32-bit little-endian words.

**Rule:** separately track bytes validated, bytes consumed (including malformed subsequences), bytes emitted and remaining source bytes. Test empty strings, trailing incomplete multibyte sequences, consecutive invalid bytes, and a valid run immediately following an invalid one. The source representation must be decoded according to the specification, not guessed from the final intended text encoding.

**Confidence:** Very high for direct John Thacker byte-accounting review and accepted follow-up fixes.

## Decode NFS filename bytes according to their specified protocol encoding

Merged João Valverde NFS correction, carried by !9205 and !9206 to maintained releases, replaces a raw `tvb_memcpy()` into a manually terminated buffer with `tvb_get_string_enc(wmem_packet_scope(), tvb, offset, length, ENC_UTF_8)` for NFSv4 directory-entry names. This makes the string-decoding boundary explicit and keeps allocation in the correct packet lifetime. The MR description also distinguishes NFSv4's expected UTF-8 from NFSv3, whose unspecified filename encoding requires more caution.

**Text-decoding rule:** select the encoding from the actual protocol/version contract, not from a convenient existing byte-copy implementation. Keep the source length bounded, use the encoding-aware TVB helper for display strings, and avoid silently projecting one protocol version's encoding onto another. **Confidence:** High for the merged fix and its exact version-aware rationale.

## Batch valid UTF-8 spans without weakening malformed-input semantics

Merged !9153 (John Thacker) replaces repeated single-byte/string-buffer appends with bulk copying contiguous validated UTF-8 bytes. The validator intentionally treats embedded NUL bytes as valid data and substitutes a single replacement character for a maximal invalid subsequence rather than applying GLib's NUL handling or naively replacing each byte. The author separated a more speculative 32/64-bit ASCII-fast-path experiment instead of merging an unmeasured cross-encoding optimization; later fixes such as !9214 remain relevant to exact consumed-byte accounting.

**Rule:** optimize the common valid run without changing how many bytes an invalid/truncated sequence consumes. Include embedded NUL, malformed continuation, trailing partial characters and non-ASCII-heavy data in tests; benchmark the actual supported compilers before adding architecture-specific scans. Guy Harris caught an MSVC C4267 warning involving size_t versus gint during review. **Confidence:** extremely high for the merged batching change, direct Guy/John discussion and portability correction.

## Arbitrary capture bytes cannot be appended directly as UTF-8 presentation text

Merged !9100 (João Valverde) repairs AllJoyn, ECMP, NCSI, ZigBee ZDP and related dissectors that inserted raw wire bytes as `%c`, manually copied unvalidated byte strings, or truncated rendered strings in fixed buffers. The accepted code uses format/escape helpers (`format_char`, printable-byte tests), `tvb_get_string_enc(..., ENC_ASCII)`, `get_utf_8_string`, packet-scoped growable buffers, and dynamically sized text formatting. Merged !9076 (John Thacker) makes the same boundary explicit for a four-byte USB-video FourCC: `tvb_format_text(..., 4)` replaces a raw NUL-terminated `%s`. These are *display* transformations; preserve the original bytes and precise source ranges for decoding.

**Rule:** a byte's ASCII-looking intent does not make its arbitrary value valid UTF-8. Decode according to the protocol's specified encoding or escape invalid bytes explicitly before passing them to UI/log strings. Avoid truncation in the middle of UTF-8 sequences, and use packet-scoped dynamic storage when the source length is not inherently small. **Confidence:** very high, independent merged fixes authored by João and John.

## pcapng string-option encoding and terminators are two separate obligations

Merged master !9035 (João Valverde) validates UTF-8 in pcapng string options, replaces malformed UTF-8 and introduces `wtap_block_add_string_option_owned()` so a prepared allocation can be transferred to a wiretap option without an extra copy. **Guy Harris** pointed to the pcapng string-option specification: options are length-delimited and need not carry a terminating NUL, but a zero octet **inside** a counted value must terminate the string. **John Thacker** observed that `g_utf8_validate()` / `g_utf8_make_valid()` treat an internal NUL as invalid and can replace it with U+FFFD, changing the semantic value rather than truncating it. Guy suggested `memchr(opt, '\\0', optlen)` and shortening the counted span before UTF-8 validation. **Caution:** the merged !9035 diff examined here does not visibly implement that suggested NUL cut-off. Treat the specification and direct maintainer review as the rule, not the merged code as a resolved exemplar; string variants such as padded strings can have different semantics.

Merged master !9024 by Gerald Combs separately corrects Qt-to-wire conversion: `QString::toLocal8Bit()` is locale-dependent on Windows and can corrupt packet comments required to be UTF-8. Use explicit `toUtf8()` at the packet-comment and other UTF-8 interface boundaries. **Guy Harris approved and requested backports**, merged as !9032 and !9033. **Confidence:** extremely high for the encoding contract and Qt fix; !9035's early-NUL implementation remains a documented caution.

## Validate transfer encodings before turning authentication or SMS bytes into text

Merged master **!8960** (John Thacker) handles CitrixAGBasic authentication by decoding Base64 but *not* assuming the decoded bytes form valid UTF-8 or ASCII. The token must be checked or escaped before presentation, and recognized user/password data is also sent to the credentials tap. Transport/transfer decoding does not establish a text-encoding guarantee.

Merged master **!8952** replaces custom CIMD character-mapping code with explicit conversion of CIMD's escaping into GSM 7-bit alphabet and then to Unicode/UTF-8 through Wireshark charset helpers. Merged **!8934** similarly uses existing `format_text()` when presenting unknown HSP/HFP commands instead of copying arbitrary bytes into a string of the same size.

**Rule:** decode the wire-defined character repertoire in its declared order (transport encoding, then character-set conversion); keep arbitrary bytes from being silently represented as valid UTF-8. Reuse shared charset/escape helpers, keep precise allocation scope, and test multibyte/invalid input. **Confidence:** very high, independently merged malformed-text fixes.

## Percent-decoding does not imply valid UTF-8

Merged HTTP/2 **!8894** validates the text returned by `g_uri_unescape_string()` before inserting it into an `FT_STRING` field, and sanitizes invalid byte sequences with `g_utf8_make_valid()`. A syntactically valid percent-escape sequence can decode to arbitrary bytes or malformed UTF-8. **Rule:** validation belongs *after* transfer/escape decoding and before tree/column presentation; do not treat a URI decoder as a character-set decoder. Check the null/error paths for the resulting generated tree item. **Confidence:** high, merged malformed-input fix.

## Prefer encoding-aware byte presentation over literal `%c` and raw-byte strings

Merged **!8890** introduces shared `format_char()` so a raw IAX2 control byte is displayed as one valid UTF-8 character or a printable C/hex escape, rather than injecting arbitrary bytes through `%c`. Merged **!8889** switches AT command names to `format_text()`; **!8888** converts reversed T.30 number bytes through `get_utf_8_string()`; **!8878** registers AOL's two-character ASCII token as `FT_STRING`, not a `FT_UINT16`; and **!8881**/**!8869** replace Locamation's raw buffer/string copying with `tvb_get_string_enc(..., ENC_UTF_8)` and `tvb_find_line_end()`.

**Rule:** match a field's storage type to its semantic domain and make its character encoding explicit. Avoid assuming that a two-byte code is a number or that a raw octet is valid display text. Reuse packet-scoped encoding helpers rather than hand formatting untrusted packet bytes. **Confidence:** very high for the merged changes.

## An unbounded UTF-8 buffer still needs a valid initialization state

Merged **!8863** replaces X.509 distinguished-name fixed-size concatenation buffers with packet-scoped `wmem_strbuf_t`, avoiding truncation partway through a multibyte character; it updates the ASN.1 template and emitted dissector together. The follow-up merged **!8868** is important: it guards `x509if_get_last_dn()` against a NULL buffer and initializes the DN buffer with an empty string rather than an inappropriate zero-size constructor.

**Rule:** dynamic string buffers solve capacity problems but not initialization, lifetime or absent-value semantics. Check accessor calls before the first DN and after the frame-end reset; review the later corrective merge rather than treating the first merge as a complete final design. **Confidence:** very high, accepted fix followed by a concrete merged crash correction.

## Validate before repairing a mostly valid UTF-8 packet string

In merged **!8792**, AFS converts RXStringV's one-octet-per-32-bit-word representation into a packet-scoped string buffer. **João Valverde** recommended first checking `wmem_strbuf_utf8_validate()` and calling `wmem_strbuf_utf8_make_valid()` only on failure; the common valid-input path avoids unnecessary sanitization. **John Thacker** agreed and identified that a validation-only caller should be allowed to pass a NULL end-pointer; merged **!8799** made that out-parameter optional without dereferencing it. The validator deliberately permits embedded NUL bytes, unlike `g_utf8_validate()`.

**Rule:** preserve the wire span separately from the converted UTF-8 string, optimize common valid input, and establish documented nullable-output semantics for shared validation APIs. If a protocol does not specify a charset, mark UTF-8 fallback as a heuristic rather than asserting the wire encoding. **Confidence:** very high; explicit João/John review followed by two merged fixes.

## Apply the governing document's character set rather than assuming raw octets are UTF-8

Merged **!8784** corrects WBXML inline and string-table entries to use the document encoding stored in packet-level protocol data; `tvb_format_text()` assumes UTF-8 and `tvb_get_ptr()` bypasses conversion entirely. Merged **!8790** decodes MMS Text-string as US-ASCII under OMA-TS-MMS-CONF and considers the charset of Encoded-string-value. Merged **!8791** converts HL7 message-type bytes with an ASCII-aware API; **!8770** uses `tvb_ws_mempbrk_pattern_guint8()` plus `tvb_get_string_enc()` for XCSL tokens. Merged **!8782** substitutes replacement characters when Skinny's ASCII/codebook displayLabel contains unrecognized high-bit bytes.

**Rule:** select encoding from the exact enclosing protocol/document rule, decode at the packet-buffer boundary, and preserve byte consumption independently of the resulting string length. Do not silently reinterpret arbitrary bytes as valid UTF-8. **Confidence:** high, corroborated by several independent merged dissector fixes.
