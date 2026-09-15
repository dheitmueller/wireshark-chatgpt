# Wireshark MR review automation ledger: !19690-!19739

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Selection method: reconstructed the already-reviewed set from the available per-run ledgers in `reviewed-mrs-automation/` plus `reviewed-mrs.md`, explicitly preserving/counting the historical !17571-!17620 batch. The corpus commit is unchanged from the preceding runs, and the next fifty highest-numbered corpus MRs not present in the reviewed set are exactly !19739 through !19690. No numeric range was inferred as reviewed merely from a partial ledger.

Exactly 50 MRs were reviewed in this run:

| MR | Review | Notes |
|---|---|---|
| !19739 | Scanned | JSON-3GPP reference update; merged, low architectural signal. |
| !19738 | Deep | Guy Harris-authored/merged data-source API cleanup: lookups return the data-source object rather than an arbitrary projection; names/signatures were aligned with actual semantics. Strong corroboration of existing API naming/contract guidance. |
| !19737 | Scanned | Zigbee ZCL Diagnostics cluster support; merged protocol-specific addition. |
| !19736 | Scanned | ORAN statistics output-width cleanup; merged, presentation-specific. |
| !19735 | Scanned | WSDG WSLua typo fix; merged documentation cleanup. |
| !19734 | Deep | Darwin legacy pcapng process information/metadata. Merged after extensive review. Strong Guy Harris/John Thacker architecture discussion on section-scoped PIB identity, section merging/remapping, extension collision policy, external-reader compatibility, and supplied pktap/droptap test captures. Added durable section-identity rule to `pcapng-extension-architecture-conventions.md`. |
| !19733 | Scanned | WSLua heuristic DissectorTable name lifetime/use-after-free fix; memory-ownership correction, no additional durable rule beyond existing lifetime guidance. |
| !19732 | Scanned | X.509 DSA/Diffie-Hellman public-key dissection; merged protocol-specific work. |
| !19731 | Scanned | Draft tap reset-listener flag for epan-memory reset; lower weight because draft/provisional. |
| !19730 | Scanned | User Guide captype CLI documentation. |
| !19729 | Scanned | TECMP 1.9 CM SerDes status support; protocol-specific. |
| !19728 | Scanned | PTPv2 management TLV dissection with supplied test capture; corroborates sample-capture validation practice. |
| !19727 | Scanned | Additional item-vs-call-length report fixes; defensive dissector cleanup, no new convention. |
| !19726 | Scanned | dumpcap multiple-output-file and swapped-endian input-pipe handling; capture-path correctness fix. |
| !19725 | Scanned | JSON-3GPP TS 29.503 feature negotiation; protocol-specific. |
| !19724 | Scanned | Range API accepts NULL as an empty range; API simplification, no additional durable rule. |
| !19723 | Scanned | WSLua heuristic DissectorTable name use-after-free fix; ownership/lifetime correction already represented by notebook guidance. |
| !19722 | Scanned | Draft falcodump plugin-version display; provisional/lower weight. |
| !19721 | Scanned | Remaining dissector assertion fixes from #17890; defensive cleanup, no new convention. |
| !19720 | Scanned | LPP media-type handle for binary LPP message representation; protocol-specific. |
| !19719 | Scanned | JSON-3GPP TS 29.572 feature negotiation; protocol-specific. |
| !19718 | Scanned (closed) | IDN typo/value-string attempt; closed/unmerged and therefore down-weighted. |
| !19717 | Scanned | JSON-3GPP SupportFeatures function split as the implementation grew; ordinary maintainability refactor. |
| !19716 | Deep | John Thacker-authored merged migration to C23-compatible checked arithmetic. Explicitly documents inverted overflow return convention, special handling required for division/modulo/negation, MSVC portability fix, and Gerald Combs SPDX correction for imported code. Strong corroboration of checked-arithmetic and third-party-source hygiene. |
| !19715 | Scanned | MessagePack support expansion and cleanup; protocol-specific. |
| !19714 | Scanned | ISAKMP IPv6 Network length correction to 16 bytes; narrow protocol fix. |
| !19713 | Scanned | Allow wslog parameters across applications after argv handling changed; CLI plumbing compatibility. |
| !19712 | Scanned | IEEE 802.11 Clang Analyzer dead-store fix; static-analysis cleanup. |
| !19711 | Scanned | Draft tap-iostat natural-loop-bounds cleanup; provisional/lower weight. |
| !19710 | Scanned | ICMP/ICMPv6 expert info for error types; merged presentation/diagnostic enhancement. |
| !19709 | Scanned | Spelling fixes; low signal. |
| !19708 | Scanned | Correct PTPv2 Management ID 0x300x range display; protocol-specific. |
| !19707 | Scanned | Persist TCP Stream Graph throughput settings via Recent settings; UI-specific. |
| !19706 | Scanned | Clang Analyzer dead-assignment fix in EBHSCR; static-analysis cleanup. |
| !19705 | Scanned | IDN dissector update to 2021 draft including audio samples; protocol-specific. |
| !19704 | Deep | Guy Harris-authored merged Zigbee conversion to `FT_ABSOLUTE_TIME` plus field-specific `TIME_VALS`; applies the generic special-time-value mechanism from !19694 and adds API-checker coverage. |
| !19703 | Scanned | falcodump missing-interface-option validation; extcap robustness fix. |
| !19702 | Scanned | LIN go-to-sleep decode correction; protocol-specific. |
| !19701 | Scanned | LIN go-to-sleep decode correction; closely related/superseding variant, no additional durable lesson. |
| !19700 | Scanned | LIN parsing centralized so pcapng, TECMP, and ASAM CMP use one implementation; good deduplication but already covered by shared-parser guidance. |
| !19699 | Scanned | ORAN FH CUS tap records section IDs; protocol/tap-specific. |
| !19698 | Scanned | ORAN eAxC ID blurb clarification; presentation-only. |
| !19697 | Scanned | IEEE 802.11 Multi-Link element field additions; protocol-specific. |
| !19696 | Deep | Guy Harris-authored/merged pcapng writer API cleanup: writers derive packet data from the authoritative record rather than receiving redundant `pd`, while `err_info` is propagated because callees may need to return diagnostics. Corroborates authoritative-source/API-domain guidance. |
| !19695 | Scanned | Ruff-driven Python cleanup; tooling maintenance. |
| !19694 | Deep | Guy Harris-authored merged `FT_ABSOLUTE_TIME` support for field-specific special values. Guy and John Thacker agreed a global NTP display mode is the wrong abstraction when zero/sentinel meanings differ by field. Added `field-value-semantics-conventions.md`. |
| !19693 | Scanned | Loop “relevant function” converted to generated item because it is interpretive metadata rather than a real field; corroborates generated-field usage. |
| !19692 | Scanned | Simple Ogg file dissector; protocol/file-format addition with known validation limitations, no broader rule extracted. |
| !19691 | Scanned | Automatic data/translation update; no substantive engineering lesson. |
| !19690 | Scanned | Automatic data/translation update; no substantive engineering lesson. |

## Durable notebook changes

1. `pcapng-extension-architecture-conventions.md`: added a rule that section-scoped object identities must remain section-scoped during save/merge operations; combining sections requires consistent remapping of definitions and all references, with external-reader compatibility explicitly validated where relevant. Primary evidence: merged !19734 with direct Guy Harris and John Thacker discussion.
2. `field-value-semantics-conventions.md`: added a rule that protocol-specific sentinel meanings belong in field-local value metadata rather than global display modes. Primary evidence: Guy Harris-authored merged !19694, direct Guy Harris/John Thacker discussion, and merged application !19704.

No other convention was promoted from this batch where the evidence merely corroborated rules already present or was too protocol-specific/provisional.