# Field Wire-Width Conventions

Merged master MR !2809 fixes independent mismatches between packet wire widths, protocol-tree item lengths, and registered integer field widths in PPCAP, Tibia, and TNEF.

Rule: the registered field width and the byte length used to decode/add the field are both part of the semantic contract. Verify both against the actual wire encoding; correct cursor movement does not make a truncated or over-wide field value correct.

Confidence: high. The merged change fixes the same class of mismatch in several dissectors.

## Treat typed-item checker width failures as protocol-specification questions

Merged master MR !2255 was triggered by tools/check_typed_item_calls.py finding a two-byte proto_tree_add_item() call for a field registered as FT_UINT8. Martin Mathieson asked for specification confirmation; John Thacker checked the DSM-CC material and confirmed privateDataLength is a 16-bit field. The accepted fix changes the registration to FT_UINT16 rather than changing the tree call merely to satisfy the checker.

**Review rule:** when the typed-item checker finds a field-width mismatch, verify the authoritative wire definition and make the registration, extraction length, and semantic C domain agree with the protocol. Do not fix the warning by forcing the call to match a stale field declaration.

**Confidence:** Very high. Merged master correction with direct specification verification during review.
