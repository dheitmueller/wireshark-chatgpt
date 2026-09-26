# Wireshark Protocol Text API Conventions

This file records durable conventions for APIs that expose protocol text to other dissectors or UI consumers. Current upstream source remains authoritative.

## Text-returning APIs should establish a safe, documented encoding boundary

A helper advertised as returning text should not hand arbitrary wire bytes to callers as though they were already a valid C/UI string. Decode or sanitize according to the protocol's text rules at the API boundary so every downstream consumer receives the same safe representation. If callers genuinely need uninterpreted bytes, expose that as a separate raw-data interface rather than weakening the text API.

Merged master MR !14692, authored and merged by John Thacker, changes `http2_get_header_value()` from returning a raw `wmem_strndup()` of HPACK field-value bytes to returning text decoded as US-ASCII through `get_ascii_string()`. Bytes outside the allowed range are represented with UTF-8 replacement characters, preventing invalid text from being inserted into protocol trees. The public API documentation was updated to state that contract and to tell callers that additional semantic decoding, such as percent decoding or RFC 8187 processing, remains their responsibility. The MR explicitly notes that a separate raw-byte accessor could be added if a future dissector legitimately needs opaque bytes.

**API rule:** make the representation promised by a text API true before returning. Apply the protocol's baseline character-set conversion and invalid-input handling centrally, document any additional caller-owned decoding, and keep opaque bytes in a distinct API/type when they are required.

**Review rule:** distinguish three layers when reviewing protocol string helpers: raw octets, baseline character decoding/sanitization, and higher-level semantic decoding. Do not let a convenience function blur raw octets into text merely because the storage type is `char *`.

**Confidence:** Very high. Merged master API hardening authored and merged by John Thacker with the encoding and caller-responsibility contract documented in the public header. This is a narrower API-boundary complement to the general packet-derived string safety guidance in `dissector-conventions.md`.

## Preserve protocol string values separately from display-label formatting

Once a string field represents decoded packet data, its protocol-tree value is part of Wireshark's semantic and filtering model. Do not run that value through a display formatter merely to escape awkward characters; doing so changes what filters and downstream consumers observe.

Merged MR !8990, authored and merged by João Valverde, strengthens the public `proto_tree_add_string()` documentation around exactly this distinction. The API accepts a custom string *value* derived from packet data. Display-oriented helpers such as `format_text()` are not an appropriate final step merely for presentation, because escaping whitespace or other special characters changes the field value, constrains later UI formatting, and makes display-filter behavior unintuitive.

This complements the encoding-boundary rule above: raw octets still need the protocol's real character decoding or sanitization before they can honestly be exposed as text. The line is between **decoding bytes into the protocol's text value** and **formatting that semantic value as a UI label**.

**Implementation rule:** decode according to the protocol's encoding contract, then store the resulting semantic string value. Leave display escaping and label formatting to presentation code unless the protocol itself defines those characters as part of the value.

**Confidence:** Very high. Merged framework/API-documentation change by João Valverde that states the intended contract directly.


## Keep generic string storage separate from the UTF-8 protocol-tree boundary

Generic string value storage and protocol/UI text do not necessarily have the same encoding contract. Silently sanitizing the generic value layer can change legal literal values and display-filter semantics; validate UTF-8 at the API boundary that actually promises text instead.

Merged master MR !8677, authored by João Valverde, removes UTF-8 sanitization from the generic ftype-string setters and performs the debug validity check when a value is inserted as a protocol-tree string. Its API documentation also states that packet-derived string values must not be escaped merely for display because doing so changes the semantic value seen by display filters. Merged !8711 is useful negative history rather than a positive exemplar: João objected after merge when IPP used its formatted label as the stored field value, and the later already-reviewed !8731/!8724 sequence supplies the accepted semantic-value/display-label correction.

**Implementation rule:** keep encoding-neutral value storage neutral. Decode/validate where an API contract requires text, and keep presentation escaping out of the semantic value.

**Confidence:** Very high. Merged framework change authored by João Valverde, reinforced by João's corrective review and the later merged IPP correction sequence.
