# Wireshark Wire Text Encoding Context Conventions

This file records durable conventions for carrying and applying protocol-declared text encodings. Current upstream source remains authoritative.

## Carry a document-selected charset through every string extraction path

When a format selects its text encoding at the document or message level, treat that encoding as parse context and use it consistently for every inline string, string-table lookup, and derived text value. Do not fall back to raw pointers or a helper whose encoding semantics happen to match only the common case.

Merged master MR !8784, authored by John Thacker, fixes WBXML by resolving the document charset to a Wireshark `ENC_*` value, retaining it as packet-level protocol data, and using `tvb_get_stringz_enc()` for both inline strings and string-table references. The MR explicitly replaces `tvb_format_text()` and `tvb_get_ptr()` paths that ignored the document-selected encoding. It also retains the WBXML default of UTF-8 when no charset is supplied.

Merged master MR !8790 applies the same principle to MMS: protocol-defined Text-string values are decoded as US-ASCII, while Encoded-string-value uses its MIBEnum charset. Merged !8791 and !8770 independently convert nominally ASCII protocol text through encoding-aware helpers before using the strings in columns or fields.

**Implementation rule:** resolve the protocol or document charset at the layer that owns it, propagate that semantic encoding through nested parsing, and materialize text with the matching `tvb_get_string*_enc()` or tree-item API. A raw byte pointer or generic text formatter is not a substitute for a known wire encoding.

## Validate UI-facing text instead of leaking out-of-domain bytes

Merged master MR !8782 fixes Skinny display labels whose protocol domain is ASCII plus a codebook. Bytes outside that domain are replaced rather than copied through as invalid UTF-8. Merged master MR !8792 similarly validates AFS vectorized strings as UTF-8 and repairs invalid input before adding the field.

**Display rule:** text that reaches columns, protocol-tree strings, JSON-like output, or other UTF-8-facing interfaces must first pass through the appropriate encoding conversion or validation path. Preserve malformed input inspectability through replacement/diagnostic behavior rather than emitting invalid UI strings.

**Confidence:** Extremely high. Multiple merged master encoding fixes, most authored by John Thacker, all converging on explicit wire-encoding semantics.

## Preserve raw bytes when the protocol does not guarantee a character set

Merged master MR !6244, authored by John Thacker, changes 802.11 SSID handling after documenting that the standard can leave the SSID octet encoding unspecified unless separate Extended Capabilities state establishes UTF-8. The accepted path preserves the SSID as raw bytes for decryption state and registers the tree field as FT_BYTES with UTF-8-printable presentation rather than forcing the bytes through an ASCII or UTF-8 validation contract that the packet does not necessarily satisfy.

The MR also records why the full discriminator is nontrivial: the relevant capability may appear later in the same frame, be absent, or be known only from prior request/conversation state. Presentation therefore remains intentionally weaker than claiming a known charset.

**Implementation rule:** distinguish "bytes that are often readable as text" from "a protocol string with a specified encoding." When the charset is genuinely unspecified, preserve the byte value as the semantic field and choose a best-effort display policy; upgrade to a typed string only when protocol context supplies a trustworthy encoding contract.

**Confidence:** Extremely high. Merged master encoding change authored by John Thacker with the standards ambiguity and state-ordering limitation documented in detail.

## Do not add byte-order semantics to plain string encodings

Merged master MR !6220, authored by João Valverde, removes redundant ENC_NA combinations from ASCII string-item calls across the tree and changes the associated fixer/checker behavior. The MR states the underlying reason directly: FT_STRING and FT_STRINGZ values have character encodings but do not have integer endianness.

**Implementation rule:** encoding flags should describe semantics the field type actually has. For ordinary string fields, specify the character encoding required by the wire format; do not combine it with a no-endianness flag merely because numeric fields use an endian position in the same API argument.

**Confidence:** Very high. Merged tree-wide cleanup and tooling correction authored by João Valverde.


## The printable-UTF-8 byte display policy was added explicitly for uncertain encodings

Merged master MR !6120, authored by John Thacker, introduces `BASE_SHOW_UTF_8_PRINTABLE` and `tvb_utf_8_isprint()` for byte-valued fields whose contents may be UTF-8 but whose protocol does not guarantee a character encoding. The change updates public headers, documentation, introspection, and the exported-symbol manifest.

This is the API foundation later applied by merged !6244 to 802.11 SSIDs. The semantic distinction is intentional: the value remains bytes; only its presentation gains a best-effort readable form.

**Confidence:** Extremely high. Merged master API work by John Thacker, subsequently used by a merged protocol fix.
