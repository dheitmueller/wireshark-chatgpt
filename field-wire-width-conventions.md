# Field Wire-Width Conventions

Merged master MR !2809 fixes independent mismatches between packet wire widths, protocol-tree item lengths, and registered integer field widths in PPCAP, Tibia, and TNEF.

Rule: the registered field width and the byte length used to decode/add the field are both part of the semantic contract. Verify both against the actual wire encoding; correct cursor movement does not make a truncated or over-wide field value correct.

Confidence: high. The merged change fixes the same class of mismatch in several dissectors.
