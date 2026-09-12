# Wireshark Protocol-Item Source-Range Conventions

This file records durable conventions for mapping protocol-tree items back to packet bytes. Current upstream source remains authoritative.

## Anchor protocol items to the bytes that actually encoded the value

A helper may decode a value through a synthetic, subset, or otherwise derived TVBuff, but protocol-tree source ranges are externally observable metadata. They drive byte highlighting and raw-output modes such as `-T jsonraw`, so an item's offset and length should identify the actual encoded bytes in the packet whenever such bytes exist.

Merged MR !22894 fixes CBOR bitmask fields whose parent item and boolean children were attached to a virtual child TVBuff instead of the CBOR integer's real encoded bytes. The accepted fix anchors the items to `chunk->start` and `chunk->head_length`; decoding and display-filter semantics remain unchanged, but raw offsets and lengths become correct.

**Implementation rule:** do not use a convenient synthetic TVBuff as an item's source merely because it simplifies value extraction. Keep decoding representation separate from source provenance, and attach protocol-tree items to the original packet range that encoded the represented value when downstream consumers expect physical offsets.

**Confidence:** High. Merged master correctness fix with explicit `jsonraw` before/after verification and maintainer merge.
