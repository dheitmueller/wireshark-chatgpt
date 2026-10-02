# Dissector Field Semantics: MRs 1710-1759

Merged !1714 records Pascal Quantin's preference for bounded tvbuff helpers and direct proto_tree_add_item() when parser logic does not separately need the field value. If the value is needed later, use the appropriate return-value tree helper rather than fetch-then-add duplication.

Merged !1756, with backports !1757 and !1758, changes a 64-bit NAS IPv6 interface identifier from FT_IPV6 to FT_BYTES with explicit formatting. The value is not a complete IPv6 address, so the full-address type could apply incorrect generic rendering.

During merged !1754, Roland Knall asks that vendor-private EPL error codes not be placed in the common standard value namespace, where another vendor could reuse the same numbers.

**Rules:** let tree APIs perform extraction when practical; choose hf types for the semantic object actually present; and keep private numeric namespaces behind an explicit vendor or extension discriminator.

**Confidence:** Very high. All referenced changes merged; !1714 and !1756 include direct protocol-maintainer guidance.
