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
