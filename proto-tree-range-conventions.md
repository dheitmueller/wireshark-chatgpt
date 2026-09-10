# Wireshark Protocol-Tree Range Conventions

This file records durable conventions for setting protocol-tree item extents. Current upstream source remains authoritative.

## Prefer an already-known end offset over redundant accumulated-length arithmetic

When a dissector already tracks the current/end offset for a variable-length item, prefer setting the item's end directly rather than separately accumulating a signed total length. Redundant length arithmetic creates another overflow domain without adding information.

Merged MR !25017, authored and merged by John Thacker after an OSS-Fuzz report, replaces accumulated WCCP item-length arithmetic with `proto_item_set_end()`. The parser already had the correct end offset; computing an additional total could trigger undefined signed overflow even though the parser otherwise knew the range.

**Implementation rule:** if the semantic endpoint is already available, use the end-based protocol-tree API. Only compute a separate total length when the length itself is required, and then prove the arithmetic domain independently.

**Confidence:** Very high. Merged master OSS-Fuzz correctness fix authored and merged by John Thacker.

## Range-adjustment offsets must refer to the field's original data source

An offset relative to one tvbuff cannot safely adjust a field backed by a different data source merely because the numeric ranges happen to line up. Preserve and enforce data-source identity when mutating protocol-item bounds.

Merged MR !25020, authored and merged by John Thacker, adds assertions to `proto_item_set_end()` and `proto_tree_set_appendix()` requiring the supplied tvbuff's data-source tvb to match the `field_info` data source (with the corresponding NULL case). The MR explicitly calls a mismatch a dissector bug.

**Implementation rule:** when an API interprets an offset relative to a tvbuff, the target field must be backed by that same underlying data source unless the API explicitly defines a cross-source translation. Treat mismatches as contract violations rather than silently rebasing numbers between unrelated buffers.

**Confidence:** Very high. Merged master API-contract enforcement authored and merged by John Thacker.