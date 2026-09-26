# Wireshark ASN.1 PER String Conventions

This file records durable conventions for PER string decoding extracted from accepted Wireshark changes. Current upstream code and the applicable ASN.1 PER specification remain authoritative.

## UTF8String is not a known-multiplier PER character string

Merged master MR !8808, authored by John Thacker, fixes `dissect_per_UTF8String()` to follow X.691 section 27.6. UTF-8 characters have variable width, so UTF8String is not a known-multiplier character string. Character-count constraints therefore are not PER-visible and cannot be used to derive a constrained bit width. The accepted implementation reads an unconstrained length determinant in octets, aligns when required by aligned PER, creates an octet-aligned TVBuff, and adds the value with `ENC_UTF_8`.

**Decoding rule:** do not apply known-multiplier restricted-character-string logic to PER UTF8String. Decode its length using the unconstrained octet-length rules and then decode the resulting octets as UTF-8.

**Constraint rule:** a character-count or permitted-alphabet constraint on a variable-width UTF8String is semantic ASN.1 information, not a fixed PER bit-width shortcut. Do not translate it into a byte/bit length assumption unless the encoding rules explicitly make that constraint visible.

## Preserve present-but-empty strings

The same MR handles zero length separately because the octet-alignment helper did not accept a zero-length input. A zero-length UTF8String is still a present value, so the dissector adds an empty string item rather than treating it as absent.

**Value rule:** distinguish protocol absence from a present value whose encoded length is zero. Helper limitations around zero-length buffers must not erase the semantic field.

**Confidence:** Extremely high. Merged master correctness change authored by John Thacker and accepted without contrary review.
