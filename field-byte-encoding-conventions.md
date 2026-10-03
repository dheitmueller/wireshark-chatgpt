# Wireshark Byte-Field Encoding Conventions

This file records durable encoding rules for raw byte-string fields.

## FT_BYTES does not use integer endianness

In merged master MR !277, Alexis La Goutte requested changing `proto_tree_add_item()` calls for `FT_BYTES` fields from `ENC_LITTLE_ENDIAN` to `ENC_NA`.

**Implementation rule:** uninterpreted octet strings have ordering as bytes, not integer byte order. Use `ENC_NA` for `FT_BYTES` unless a specific API or field representation defines a separate string/character encoding contract. Reserve `ENC_LITTLE_ENDIAN` and `ENC_BIG_ENDIAN` for field types whose semantic value depends on integer byte order.

**Confidence:** High. Direct maintainer review incorporated into a merged master protocol extension.
