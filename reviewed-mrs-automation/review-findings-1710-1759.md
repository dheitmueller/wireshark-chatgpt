# Findings: Wireshark MRs 1710-1759

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054. All 50 reviewed MRs are merged.

Strong reusable evidence:
- !1741: IPv4 reassembly should not blindly include VLAN identity; private/link-local reuse can justify stricter disambiguation.
- !1730: PCI-ID support was steered toward a tools/make-* refresh generator, private implementation types, project hooks, and clean history.
- !1731 and !1732: check descriptor duplication failure and close the duplicate if stream-wrapper creation fails.
- !1740: packet sizes are semantically unsigned, but signed textual parsing can be retained long enough to reject negative input before conversion.
- !1714: use bounded tvbuff helpers and add directly from the tvbuff when no separately fetched field value is needed.
- !1710: SOME/IP subdissectors receive explicit parent-message context.
- !1756 through !1758: a 64-bit IPv6 interface identifier is not a full IPv6 address; use the truthful field type and explicit display formatting.
- !1754: vendor-private codepoints must not be folded into a shared standard value namespace.

The remaining MRs mainly reinforce existing C/static-analysis cleanup, backport discipline, generated-data updates, expert-info documentation, sample-capture expectations, and protocol-specific fixes. No ST 291/VANC packet type was encountered.
