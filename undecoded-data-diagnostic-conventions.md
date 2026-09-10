# Wireshark Undecoded Data Diagnostic Conventions

This file records durable protocol-tree diagnostic guidance extracted from accepted upstream Wireshark review. Current upstream dissectors remain authoritative.

## Anchor undecoded-data diagnostics to the actual undecoded bytes

When bytes are structurally present but not decoded, expose those bytes as a protocol-tree field and attach the expert diagnostic to that field item. Attaching the warning only to a broad parent subtree loses the precise packet range and makes it harder for users to see exactly which input Wireshark could not interpret.

During merged MR !24760, Stig Bjørlykke reviewed the new DECT NR CVG dissector and directed the contributor to add the undecoded extent with `proto_tree_add_item(..., ENC_NA)` and then call `expert_add_info()` on the returned item. The same review distinguished genuinely undecoded content from reserved values that can simply fall through the value mapping without special expert handling.

**Diagnostic rule:** for known byte ranges that Wireshark cannot semantically decode, add a raw/undecoded bytes field spanning exactly that range and attach `PI_UNDECODED`-style expert information to the item representing those bytes. Do not manufacture an expert indication for every reserved enumeration value merely because it lacks a dedicated value-string entry.

**Confidence:** Very high. Direct review guidance from Stig Bjørlykke in a heavily reviewed dissector addition that was subsequently merged.
