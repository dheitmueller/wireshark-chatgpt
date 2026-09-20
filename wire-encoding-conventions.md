# Wireshark Wire-Encoding Conventions

This file records durable conventions for representing packet fields whose wire components have independent encoding rules. Current upstream source remains authoritative.

## Split composite helpers when their subcomponents require different encodings

A convenience field type is only correct when its complete wire-format contract matches the protocol. Length-prefixed strings, counted arrays, and similar composite fields can contain subcomponents whose byte order or encoding is independent. Do not force them through one helper merely because it can consume the complete byte span in one call.

Merged master MR !15671, authored and merged by John Thacker, corrects MySQL result-set strings. Their length prefix is always little-endian, while the string payload can use a character set whose Wireshark encoding is big-endian UTF-16 or UTF-32. `FT_UINT_STRING` cannot express one endianness for the integer length and another for the text, so the accepted implementation separates the wire representation into an `FT_UINT24` length and an `FT_STRING` value and applies the appropriate encoding to each.

**Implementation rule:** before using a composite `FT_*` type or combined extraction helper, verify that all encoded parts share the helper's assumptions about byte order, character encoding, width, and termination. If the protocol gives the prefix and payload independent representation rules, dissect them as independent fields/components and preserve their separate semantics.

**Review rule:** inspect both the common encoding and the less common alternatives. A composite helper can appear correct for UTF-8 or other byte-oriented text while failing only when a negotiated encoding makes payload endianness significant.

**Confidence:** Very high. Merged master correctness change authored and merged by John Thacker, with the incompatible length-prefix/text encoding contract stated explicitly in the MR.