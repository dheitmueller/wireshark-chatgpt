# Tree-Independent Dissection Conventions

## Semantic parsing must survive a null protocol tree

The protocol tree is a presentation product. Wireshark can perform a first pass without constructing that tree, while transaction state, validation results, offsets, and other semantic effects may still be required for redissection and later consumers.

Merged master MR !564 fixes CLASSIC-STUN by removing a broad `if (tree)` guard around transaction correlation and attribute dissection. The complete packet is now semantically processed on the first pass even when no tree is rendered.

Closed MR !562 supplies direct core-API review evidence. It proposed moving `CHECK_FOR_NULL_TREE(tree)` ahead of field-length calculation and validation in `proto_tree_add_item_new()`. Pascal Quantin pointed out that the first pass can have no tree and that this would prevent the exception/validation path from running; Anders Broman agreed and abandoned the change.

**Implementation rule:** use `tree` checks only for work that is truly presentational. Parsing, protocol validation, state transitions, correlation, and any result required by later passes must not depend on tree availability.

**Core-API rule:** do not move null-tree fast paths ahead of semantic bounds or length validation unless the API explicitly defines those checks as presentation-only.

**Confidence:** Very high. Merged protocol fix plus explicit Pascal Quantin/Anders Broman negative review of the corresponding core mistake.
