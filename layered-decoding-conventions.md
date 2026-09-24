# Wireshark Layered-Decoding Conventions

This file records durable conventions for representation changes performed while one dissector hands data to another. Current upstream dissector contracts remain authoritative.

## Apply each wire-to-logical transformation exactly once

Escaping removal, de-stuffing, decompression, decryption, and similar representation changes need a single owning layer. Once a parent dissector has converted wire bytes into the logical representation promised to a child, the child must consume that representation as-is unless its own protocol layer defines an additional distinct transformation. Repeating the same normalization at both layers silently corrupts values that legitimately contain the escape byte or pattern.

Merged MR !11567 fixes Telnet AUTHENTICATION for Kerberos V5. Telnet option subnegotiation had already converted doubled IAC bytes before calling the authentication parser, but the Kerberos-specific path retained an older identical unescape step. Four consecutive `0xff` bytes were therefore reduced first to two bytes and then incorrectly to one. The RFCs provide no second escaping layer, so the accepted fix removes the Kerberos-side unescape and passes a subset tvbuff of the already-normalized Telnet option data directly to the Kerberos dissector. Anders Broman approved and merged the change.

**Implementation rule:** document or infer the representation contract at every subdissector boundary: raw wire bytes, framing-normalized bytes, decrypted bytes, decompressed bytes, and so on. A child must not repeat a transformation already guaranteed by its caller merely because historical code once received a less-processed representation.

**Refactoring rule:** when moving a transformation to a common parent layer, audit all protocol-specific child paths for legacy copies of the same operation. Centralization is incomplete until duplicate child normalization is removed.

**Testing rule:** use inputs containing repeated escape/control values whose result differs after one transformation versus two. Ordinary payloads that contain no escape byte cannot detect a double-normalization regression.

**Confidence:** Very high. The failure mechanism is explicit, the corrected behavior follows the relevant Telnet/Kerberos layering, and the merged master fix was approved by Anders Broman.