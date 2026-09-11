# Wireshark Dissector-Table Registration Conventions

This file records durable rules about registering and matching dissector-table keys. Current upstream source remains authoritative.

## Match table key semantics to the protocol, not to an accidental canonical spelling

When the protocol defines a textual discriminator as case-insensitive, register the corresponding string dissector table with case-insensitive matching rather than requiring every producer and every dissector registration to normalize manually to one spelling. Preferred/canonical presentation case does not imply case-sensitive matching.

Merged MR !24157 changes the HTTP Upgrade dissector table from `STRING_CASE_SENSITIVE` to `STRING_CASE_INSENSITIVE`, citing RFC 9110's requirement that protocol-name tokens are case-insensitive. The motivation also notes that registrations had already used preferred-case spellings, making a lowercase-only convention easy to violate. Anders Broman approved and merged the change.

**Registration rule:** derive `STRING_CASE_SENSITIVE` versus `STRING_CASE_INSENSITIVE` from the wire protocol's comparison semantics. Do not encode an undocumented normalization convention in callers when the table itself can express the correct matching rule.

**Confidence:** Very high. Small merged master change directly aligning the generic registration mechanism with normative protocol semantics.