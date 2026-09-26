# Wireshark Protocol-Item Source-Range Conventions

This file records durable conventions for mapping protocol-tree items back to packet bytes. Current upstream source remains authoritative.

## Anchor protocol items to the bytes that actually encoded the value

A helper may decode a value through a synthetic, subset, or otherwise derived TVBuff, but protocol-tree source ranges are externally observable metadata. They drive byte highlighting and raw-output modes such as `-T jsonraw`, so an item's offset and length should identify the actual encoded bytes in the packet whenever such bytes exist.

Merged MR !22894 fixes CBOR bitmask fields whose parent item and boolean children were attached to a virtual child TVBuff instead of the CBOR integer's real encoded bytes. The accepted fix anchors the items to `chunk->start` and `chunk->head_length`; decoding and display-filter semantics remain unchanged, but raw offsets and lengths become correct.

**Implementation rule:** do not use a convenient synthetic TVBuff as an item's source merely because it simplifies value extraction. Keep decoding representation separate from source provenance, and attach protocol-tree items to the original packet range that encoded the represented value when downstream consumers expect physical offsets.

**Confidence:** High. Merged master correctness fix with explicit `jsonraw` before/after verification and maintainer merge.

## Do not assign current-packet bytes to values derived from earlier protocol state

A protocol-tree value can be semantically valid for the current packet even when no bytes in that packet encode it. In that case, giving the item an arbitrary nonzero TVBuff range is misleading: byte highlighting and raw-output consumers will claim that unrelated bytes are the source of the value.

Merged master MR !9846, authored by Guy Harris, fixes Gryphon response dissection where the IOCTL code displayed in a response is recovered from the corresponding request rather than read from the response packet. The accepted change adds that derived value with a zero offset and zero length instead of attaching it to one byte in the response. It also corrects the field from `FT_UINT8` to `FT_UINT32`, matching the stored IOCTL value. The same correction was accepted on release-4.0 in !9861 and release-3.6 in !9862.

**Implementation rule:** if a displayed field is generated, inferred, or recovered from prior conversation/request state and has no encoding in the current TVBuff, do not fabricate a source byte range for it. Use the appropriate generated/zero-length representation so highlighting and raw exports do not attribute unrelated bytes to the value.

**Review rule:** audit source provenance separately from field value type. A derived field can need a wider semantic type while simultaneously having no current-packet byte extent.

**Confidence:** Extremely high. Merged master fix authored by Guy Harris with accepted backports to both maintained release branches.

## Source extent and semantic string extent may intentionally differ

A protocol item's packet-byte range answers which bytes constitute the encoded field for highlighting/provenance; the decoded string value answers what semantic text the field contains. Those extents do not always have to be identical.

During review of merged master MR !8877, John Thacker explicitly distinguished two line-oriented cases. If the end-of-line octet is meant to be part of the string value, decode the range including it. If the protocol wants the EOL included in the highlighted field bytes but not in the semantic string, keep the decoded string length excluding the terminator and extend the protocol item's source range to the terminator.

**Implementation rule:** decide separately whether delimiter/framing bytes belong to the field's encoded source extent and whether they belong to its semantic value. Do not change the stored string merely to obtain the desired byte highlighting, and do not shrink source provenance merely because presentation omits a delimiter.

**Confidence:** Very high. Merged master change with explicit review clarification from John Thacker about both behaviors.
