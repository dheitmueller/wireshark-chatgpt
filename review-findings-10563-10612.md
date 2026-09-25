# Review findings: Wireshark MRs !10563–!10612

Corpus: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly 50 previously unreviewed MRs, descending from !10612 through !10563. The batch contains 49 merged MRs and one closed/unmerged draft (!10604). Merged master changes were treated as the strongest evidence; stable backports mainly corroborate their master changes. Maintainer-authored/reviewed work by John Thacker, Martin Mathieson, Anders Broman, Alexis La Goutte, Gerald Combs, and João Valverde received appropriate weight. This batch contains no substantive Guy Harris review discussion, so no Guy-specific authority was inferred.

## Promoted durable findings

- **!10611 — negotiated protocol state and redissection:** John Thacker's merged TDS change records client and server version advertisements separately, derives the jointly supported version, and mutates conversation version only on the first pass. Added to `protocol-version-inference-conventions.md`.
- **!10605 — checker remediation requires whole-field auditing:** Martin Mathieson's review explicitly rejects changing a field merely to satisfy one checker report when the same `hf_` symbol is used by wider accesses elsewhere. Added to `typed-item-checker-conventions.md`.
- **!10588 with !10591/!10592 — parser return coordinate systems:** a helper returning an absolute next offset must not return a relative remaining length on error. Added to `parser-return-contract-conventions.md`.
- **!10584 — filterable fields versus composite presentation:** BGP labeled VPN NLRI was split into real prefix-length, label-stack, Route-Distinguisher, and IPv6-prefix fields so TShark can extract the semantics instead of seeing only a formatted label. Added to `protocol-tree-hierarchy-conventions.md`.
- **!10565 — stable identity for repeated protocol layers:** TLS-over-TLS state now uses the occurrence number of TLS itself rather than the absolute protocol-stack layer number, which can change between first pass and redissection. The MR also adds ordinary and two-pass regression tests. Added to `protocol-layer-accounting-conventions.md`.

## Strong corroboration retained without duplicate rules

- **!10583, !10566, !10574:** string dissector-table equality is a semantic property of the protocol. !10583 introduced the explicit `STRING_CASE_SENSITIVE` / `STRING_CASE_INSENSITIVE` modes; !10566 applies case-insensitive matching to media types per RFC 6838, with !10574 as the stable backport. Existing dissector-table registration guidance already captures this.
- **!10598:** Alexis La Goutte's request to use a display-and-return proto-tree helper reinforces the existing avoid-double-fetch pattern.
- **!10579:** a fuzzed non-ASCII WBXML timezone byte is rendered through Wireshark's safe character formatter rather than injected with `%c`, reinforcing packet-derived text safety.
- **!10606/!10607:** Qt objects whose lifetime follows another QObject should express that ownership through parent construction where possible; existing Qt lifetime notes already cover the general rule.
- **!10608 and !10575:** cleanup APIs must be read carefully for what they actually own; a helper that frees internal members is not necessarily the destructor for the enclosing allocation.
- **!10569:** child parsing must receive the intended subtree, corroborating existing protocol-tree hierarchy guidance.
- **!10570/!10578:** test-runner integration should provide actionable per-test failure output; these are useful infrastructure examples but not distinct architecture rules.

## Lower-weight / non-promoted evidence

- **!10604** is a closed WIP proposal to load every TLS keylog file from a configured directory. Peter Wu explicitly said the approach would not be merged in its current form because of maintenance concerns and invited a redesigned revision. It is retained only as negative/contextual evidence.
- Automatic update, dependency refresh, documentation/package, and narrow build-system MRs in this batch were scanned but did not justify new durable conventions.

The exact reviewed set and per-MR disposition are recorded in `reviewed-mrs-automation/reviewed-mrs-automation-10563-10612.md`.
