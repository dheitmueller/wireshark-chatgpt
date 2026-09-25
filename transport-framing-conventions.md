# Wireshark Transport Framing Conventions

This file records reusable conventions for framing protocol data carried by stream and message transports.

## TCP is a byte stream, not a PDU transport

- Do not assume that one TCP segment contains exactly one application PDU. A single application PDU may span multiple TCP segments, and a single TCP segment may contain multiple complete application PDUs.
- When a TCP-carried protocol has a length field or otherwise permits the next PDU boundary to be determined, prefer Wireshark's standard `tcp_dissect_pdus()` pattern rather than hand-rolling segment-boundary handling.
- The normal structure is a minimum fixed-header length, a callback that returns the complete application-PDU length once enough header bytes are available, and a per-PDU dissector callback. This lets the TCP machinery request desegmentation when necessary and repeatedly decode multiple PDUs from one TCP payload.
- Keep framing policy transport-specific. A protocol carried over message-oriented transports may already receive natural message boundaries, while its TCP binding needs explicit stream framing.

Merged MR !21993 is strong evidence for this convention. The NetPerfMeter dissector failed both when one message was split across TCP packets and when multiple short messages appeared in one packet. Jaap Keuter identified the missing PDU-boundary handling and explicitly directed the contributor to `tcp_dissect_pdus()` and `doc/README.dissector` section 2.7.1; the contributor reworked the MR accordingly and it subsequently merged.

## Preserve helper success contracts

- A helper that returns success and also returns data through output parameters must satisfy those output postconditions on every successful path. Do not return success after recognizing an input form if no usable output object/value was actually produced.

Merged MR !22037 fixes Kafka Snappy decompression that could return true for an Xerial-formatted input containing no chunks while leaving decompression outputs unset. The accepted fix reports success only when a composite TVB was actually created, while the ordinary decompression path sets its output before returning true.

## Reuse canonical representation helpers

- When Wireshark already has a canonical formatter/serializer for a value that will be embedded in a display-filter expression, use it rather than reproducing the syntax locally. This keeps edge cases aligned with the field type and makes later representation changes propagate automatically.

Merged release MR !21994 replaces a hand-built hexadecimal byte-string display-filter expression with the existing `bytes_to_dfilter_repr()` implementation. Besides removing duplicate formatting code, it fixes the one-byte case and guarantees consistency with other `FT_BYTES` display-filter rendering.


## Separate reusable PDU dissection from transport-specific framing

When the same protocol PDU can arrive through more than one carrier, keep the semantic PDU decoder separate from the transport wrapper. A TCP-facing entry point may own stream desegmentation and repeated-PDU iteration, while an embedding protocol that already supplies complete boundaries should be able to call a one-PDU entry point directly. The per-PDU decoder should consume only its own PDU and, when useful to its caller, return the number of bytes consumed rather than assuming it owns all remaining bytes in the tvbuff.

Merged master MR !10460, authored by John Thacker and merged by Alexis La Goutte, applies this directly to BGP carried inside BMP. The ordinary BGP dissector is TCP-oriented and performs stream framing/desegmentation, but BMP already provides complete, aligned BGP PDUs inside a larger BMP message. The accepted change registers a separate one-PDU BGP handle, has BMP call it for the embedded messages, makes the PDU routine return the bytes it consumed so BMP can continue to optional trailing TLVs, and sets the BGP protocol-item length to the actual embedded PDU rather than all bytes supplied by the parent.

Merged master MR !10423, also authored and merged by John Thacker, provides complementary evidence for alternate wire representations: H.265 gains a named Annex-B bytestream entry point that MPEG-PES can call through a registered dependency rather than forcing a transport-specific path to serve a representation it does not own.

**Architecture rule:** expose transport/representation-specific wrappers around a reusable semantic decoder when a protocol is carried in materially different framing contexts. Let the layer that knows the framing own reassembly and PDU boundaries; do not make an embedded caller emulate TCP merely to reach the decoder.

**Confidence:** Very high. Both examples are merged master changes authored by John Thacker; !10460 was independently accepted by Alexis La Goutte.
