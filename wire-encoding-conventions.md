# Wireshark Wire-Encoding Conventions

This file records durable conventions for representing packet fields whose wire components have independent encoding rules. Current upstream source remains authoritative.

## Split composite helpers when their subcomponents require different encodings

A convenience field type is only correct when its complete wire-format contract matches the protocol. Length-prefixed strings, counted arrays, and similar composite fields can contain subcomponents whose byte order or encoding is independent. Do not force them through one helper merely because it can consume the complete byte span in one call.

Merged master MR !15671, authored and merged by John Thacker, corrects MySQL result-set strings. Their length prefix is always little-endian, while the string payload can use a character set whose Wireshark encoding is big-endian UTF-16 or UTF-32. `FT_UINT_STRING` cannot express one endianness for the integer length and another for the text, so the accepted implementation separates the wire representation into an `FT_UINT24` length and an `FT_STRING` value and applies the appropriate encoding to each.

**Implementation rule:** before using a composite `FT_*` type or combined extraction helper, verify that all encoded parts share the helper's assumptions about byte order, character encoding, width, and termination. If the protocol gives the prefix and payload independent representation rules, dissect them as independent fields/components and preserve their separate semantics.

**Review rule:** inspect both the common encoding and the less common alternatives. A composite helper can appear correct for UTF-8 or other byte-oriented text while failing only when a negotiated encoding makes payload endianness significant.

**Confidence:** Very high. Merged master correctness change authored and merged by John Thacker, with the incompatible length-prefix/text encoding contract stated explicitly in the MR.

## Do not confuse byte ordering with semantic bit significance

Reordering bytes into a big-endian or little-endian representation does not by itself reverse the protocol-defined meaning of individual bits within the resulting field. Byte order answers how bytes are assembled into a value; the specification's bit assignments answer which bit positions carry which semantics. Treat those as separate questions.

Merged master MR !15223, authored by John Thacker and merged by Alexis La Goutte, fixes the IEEE 1905 HE-MCS set. The field is reordered from its 802.11 representation into big-endian byte order, but that transformation does not make the Max HE-MCS for 1 spatial stream become the MSB or the 8-spatial-stream field become the LSB. The accepted masks preserve the semantic bit assignments. Release backport !15224 carries the same fix. During review, John also distinguished mask correctness from presentation order: listing the MSB field first was desirable for consistency with most Wireshark bitmask trees, but that display order was not what determined the masks.

**Implementation rule:** when a protocol field is byte-swapped, normalized, or read with an explicit endianness, derive bit masks from the specification's semantic bit numbering after establishing exactly what numeric value the API returns. Do not reverse masks merely because the bytes were reversed.

**Review rule:** verify three things independently for multi-bit fields: wire byte order, numeric bit positions after decoding, and tree presentation order. A patch can make the byte sequence look right while silently swapping the meanings of the LSB and MSB fields.

**Confidence:** Very high. Merged master correctness fix authored by John Thacker, maintainer-reviewed, and independently carried to a release branch.

## Use one runtime encoding decision for every access to the same field

When byte order varies with encapsulation, capture source, negotiated mode, or another runtime condition, the chosen encoding is part of the field's decoding state. A tree helper and a separate manual fetch of the same bytes must consume that same decision; hard-coding one of those paths to little- or big-endian can make displayed fields and derived summaries disagree.

Merged master MR !14372, authored and merged by Guy Harris, fixes SocketCAN CAN XL by replacing a fixed `tvb_get_letohl()` fetch with `tvb_get_guint32(..., xl_encoding)`. The same `xl_encoding` is already used for the corresponding tree fields and resolves to little-endian for `LINKTYPE_CAN_SOCKETCAN` but host-endian for Linux cooked captures. Stable-branch counterpart !14373 carries the same correction.

**Implementation rule:** compute the encoding once at the point where the format/encapsulation distinction is known and pass that encoding consistently to tree-add helpers, manual `tvb_get_*` reads, validation, and any derived-value calculations. Do not duplicate the condition with a second hard-coded accessor.

**Review rule:** whenever a dissector has an `encoding` variable or separate entry points for multiple byte-order domains, search for manual accesses to the same header. Verify that all of them use the same runtime decision, including values used only for columns or appended tree text.

**Confidence:** Extremely high. Merged master correctness fix authored and merged by Guy Harris, with an accepted stable-branch counterpart.

## Scope byte-order compatibility preferences to the exact wire contract they repair

A byte-swapping preference is not a generic permission to reverse every related header. Such preferences usually encode a very specific historical producer or capture-library quirk. Applying the same toggle to a newer record type with different byte-order semantics can create a new bug even when both formats belong to the same protocol family.

Merged master MR !14362, authored and merged by Guy Harris, stops applying SocketCAN's classic/CAN FD ID-and-flags byte-swap preference to CAN XL. The MR explains that CAN XL has a different capture-path byte-order contract and would need a separate preference if a real compatibility case ever requires one. The accepted change also renames the existing preference so its user-facing text explicitly says that it controls the classic/CAN FD ID/flags field. Release-4.2 backport !14363 confirms the policy.

**Implementation rule:** tie a byte-order compatibility preference to the smallest wire representation and historical producer behavior it actually describes. Do not extend it to a new protocol generation merely because the code paths are adjacent or the old preference happens to produce plausible bytes. If the new format develops its own compatibility problem, model that separately.

**Review rule:** for every byte-swap preference, identify the precise encapsulation, field set, producer/library behavior, and versions it represents. Make the preference label/help text expose that scope, and verify that newer header formats are not accidentally controlled by it.

**Confidence:** Extremely high. The master change and stable backport were authored and merged by Guy Harris, and the MR rationale explicitly separates the classic/CAN FD and CAN XL byte-order contracts.