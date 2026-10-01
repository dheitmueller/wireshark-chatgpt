# Packed Identifier Key Conventions

Merged master MR !2806 fixes CAN-based AUTOSAR NM and Signal PDU matching where the numeric CAN identifier shares storage with non-identity flag bits.

Rule: normalize a protocol identifier to its identity bits before using it for comparison, hashing, table lookup, or state correlation. Framing flags stored in the same scalar are not part of logical identity.

Confidence: high. The same merged correction applies the rule in two CAN consumers.
