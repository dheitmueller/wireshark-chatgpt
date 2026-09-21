# Wireshark Protocol Text API Conventions

This file records durable conventions for APIs that expose protocol text to other dissectors or UI consumers. Current upstream source remains authoritative.

## Text-returning APIs should establish a safe, documented encoding boundary

A helper advertised as returning text should not hand arbitrary wire bytes to callers as though they were already a valid C/UI string. Decode or sanitize according to the protocol's text rules at the API boundary so every downstream consumer receives the same safe representation. If callers genuinely need uninterpreted bytes, expose that as a separate raw-data interface rather than weakening the text API.

Merged master MR !14692, authored and merged by John Thacker, changes `http2_get_header_value()` from returning a raw `wmem_strndup()` of HPACK field-value bytes to returning text decoded as US-ASCII through `get_ascii_string()`. Bytes outside the allowed range are represented with UTF-8 replacement characters, preventing invalid text from being inserted into protocol trees. The public API documentation was updated to state that contract and to tell callers that additional semantic decoding, such as percent decoding or RFC 8187 processing, remains their responsibility. The MR explicitly notes that a separate raw-byte accessor could be added if a future dissector legitimately needs opaque bytes.

**API rule:** make the representation promised by a text API true before returning. Apply the protocol's baseline character-set conversion and invalid-input handling centrally, document any additional caller-owned decoding, and keep opaque bytes in a distinct API/type when they are required.

**Review rule:** distinguish three layers when reviewing protocol string helpers: raw octets, baseline character decoding/sanitization, and higher-level semantic decoding. Do not let a convenience function blur raw octets into text merely because the storage type is `char *`.

**Confidence:** Very high. Merged master API hardening authored and merged by John Thacker with the encoding and caller-responsibility contract documented in the public header. This is a narrower API-boundary complement to the general packet-derived string safety guidance in `dissector-conventions.md`.
