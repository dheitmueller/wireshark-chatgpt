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

## When the final PDU extent is not yet known, start broad and shrink

Some analysis consumers observe protocol-item extents while dissection is in progress. If a dissector cannot know the exact PDU length when it creates its top-level protocol item, initialize that item to a conservative enclosing TVBuff span and later reduce it once the true end is known. Do not begin with a short guessed extent and expand it as parsing advances.

Merged MR !22950, authored by Jaap Keuter and merged by Michael Mann, explicitly standardizes this pattern across the remaining dissectors that behaved otherwise. Its rationale states that analysis engines such as MATE depend on uncertain-length dissectors first declaring the enclosing TVB and then reducing the protocol-item scope.

**Implementation rule:** treat an initially unknown protocol-item extent as an upper bound that parsing refines downward. Prefer shrink-only range refinement over incremental expansion when downstream analysis may inspect the tree during dissection.

The same review also illustrates scope discipline: Anders Broman suggested folding in a related unsigned-offset idiom, and Jaap deferred that to a separate broad change so the mechanical API migration would remain visible rather than being hidden inside the semantic range fix.

**Confidence:** Very high. Merged master behavioral cleanup with the analysis-engine dependency stated directly by the author and accepted by Michael Mann.