# Wireshark MR review automation ledger: !25559–!25608

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Review direction: newest to oldest. This run reviewed exactly 50 previously unreviewed merge requests: !25608 down through !25559 inclusive.

| MR | Review depth | Outcome / durable note |
|---|---|---|
| !25608 | Scanned | Merged removal of obsolete transitional compatibility code; no new durable convention. |
| !25607 | Scanned | Merged conversation-flags documentation/comment cleanup; no new durable convention. |
| !25606 | Scanned | Merged PFCP specification update; no substantive reusable review lesson. |
| !25605 | Scanned | Merged XnAP generated/specification update; no substantive reusable review lesson. |
| !25604 | Scanned | Merged NGAP generated/specification update; no substantive reusable review lesson. |
| !25603 | Scanned | Merged F1AP generated/specification update; no substantive reusable review lesson. |
| !25602 | Discussion-focused | Merged O-RAN update; Jaap Keuter suggested representing the reserved command range rather than a misleading singleton. Useful local field-modeling review, but not promoted as a new general rule. |
| !25601 | Scanned | Merged NR RRC generated/specification update; no new durable convention. |
| !25600 | Deep | Merged editcap SCTP-split truncation fix. Copies only bytes actually captured while preserving original packet length semantics; strong corroboration of existing `caplen` versus `len` guidance. |
| !25599 | Discussion-focused | Merged warning cleanup. Review clarified that `&rec->key` is non-NULL even when the contained pointer is NULL; no new general convention. |
| !25598 | Deep | Merged John Thacker deterministic-output fix adds sorted wmem-map key retrieval because randomized hash iteration destabilized DNS regression output. Promoted to `container-api-conventions.md`. |
| !25597 | Scanned | Merged proto API signed-to-unsigned length cleanup; corroborates existing semantic integer-domain guidance. |
| !25596 | Scanned | Merged project-wide AI trailer rename to `Assisted-by:`. Notebook already records the newer policy/enforcement convention, so no duplicate promotion. |
| !25595 | Deep | Merged HTTP malformed request-target diagnostics. Jaap Keuter caught an inappropriate `visited` guard around expert info; warning must be regenerated on redissection. Promoted to `dissector-context-flow-conventions.md`. |
| !25594 | Scanned | Merged JSON personal-dictionary precedence feature; personal configuration loads after and overrides system dictionary. No new general rule promoted. |
| !25593 | Deep | Merged Guy Harris interface cleanup passes the short-lived IEEE 802.15.4 decryption key explicitly instead of storing it in broader context state. Promoted to `allocator-scope-conventions.md`. |
| !25592 | Scanned | Merged SGP.32 specification update; no reusable review lesson. |
| !25591 | Scanned | Merged LPP generated/specification update; no reusable review lesson. |
| !25590 | Scanned | Merged LTE RRC generated/specification update; no reusable review lesson. |
| !25589 | Scanned | Merged NAS 5GS specification update; no reusable review lesson. |
| !25588 | Deep | Merged Windows capture-options fix avoids `_stat` for named-pipe classification because probing can connect/disconnect the pipe. Strong platform-specific I/O semantics example; not promoted separately to avoid over-generalizing one API. |
| !25587 | Scanned | Merged NAS EPS specification update; no reusable review lesson. |
| !25586 | Scanned | Merged SIP Route-header cleanup/fix; no new durable convention. |
| !25585 | Scanned | Merged Clang-analyzer error-path leak cleanup; corroborates existing cleanup/ownership guidance. |
| !25584 | Deep | Merged ciscodump hostile-input hardening bounds packet writes to `PACKET_MAX_SIZE`; corroborates existing capacity/bounds rules. |
| !25583 | Scanned | Merged release-4.6 DNSKEY naming backport; no additional lesson beyond master. |
| !25582 | Scanned | Merged release-4.4 DNSKEY naming backport; no additional lesson beyond master. |
| !25581 | Scanned | Merged master DNSKEY flag terminology correction and requested backports; no new general convention. |
| !25580 | Scanned | Merged DNSSEC Key ID-to-Key Tag terminology consistency update; no substantive human review lesson. |
| !25579 | Deep | Merged Guy Harris repair of static-analysis-induced IEEE 802.15.4 key-pointer crash. Safe temporary stack-pointer lifetime was later improved by !25593; treated as evolution evidence, not preferred final API. |
| !25578 | Scanned | Merged release-4.4 backport of small-negative signed-time formatting fix; no additional lesson. |
| !25577 | Scanned | Merged release-4.6 backport of small-negative signed-time formatting fix; no additional lesson. |
| !25576 | Deep | Merged large-time formatting overflow fix decomposes negative values before negation and preserves wide type formatting; corroborates existing arithmetic-safety guidance. |
| !25575 | Scanned | Merged SCTP DTLS KM method-ID update; no reusable review lesson. |
| !25574 | Scanned | Merged Coverity-driven MD5 digest-array zero initialization; basic defensive initialization, already covered by broader safety guidance. |
| !25573 | Scanned | Merged master small-negative signed-time formatting fix; no new general convention. |
| !25572 | Deep | Merged NetScaler writer zero-initializes complete signature structs before output, preventing uninitialized fields/padding from leaking into written records; corroborates existing wire-layout/initialization guidance. |
| !25571 | Scanned | Merged release-4.6 backport of TLS hash-context failure handling; master !25560 carries the stronger evidence. |
| !25570 | Scanned | Merged automatic numbers/translations update on master; no reusable review lesson. |
| !25569 | Scanned | Merged automatic update on release-4.6; no reusable review lesson. |
| !25568 | Scanned | Merged automatic update on release-4.4; no reusable review lesson. |
| !25567 | Scanned | Merged DNSSEC algorithm-number registry update; no reusable review lesson. |
| !25566 | Scanned | Merged Guy Harris indentation correction; no new durable convention. |
| !25565 | Discussion-focused (open snapshot) | QMux dissector with extensive tests. Jaap Keuter requested folding a private header into the C file because it exposed no interface; Alexis otherwise found it good. Still open/unmerged in the corpus snapshot, so architectural evidence remains provisional. |
| !25564 | Deep | Merged release-4.6 Guy Harris Windows Search lifetime fix allocates names at file scope when retained persistently and sanitizes display strings; corroborates existing persistent-container ownership guidance. |
| !25563 | Scanned | Merged Guy Harris cleanup uses the correct unsigned format and corrects an ownership comment to match caller-provided storage; useful hygiene but no new rule. |
| !25562 | Scanned | Merged release-4.4 SSH keylog constructor-failure checks for invalid lengths; backport of master fix, no additional rule. |
| !25561 | Scanned | Merged release-4.6 SSH keylog constructor-failure checks; no additional rule beyond master. |
| !25560 | Deep | Merged TLS hash-context lifecycle fix checks initialization failure and uses non-failing reset rather than cleanup/re-init when restarting the same algorithm, reducing failure/double-free states. Strong implementation exemplar; no separate notebook rule promoted this run. |
| !25559 | Deep | Merged JSON+ path-based protocol dispatch prefers the more specific path discriminator and falls back to generic JSON when configured protocol rules do not match, rather than applying the wrong dictionary. Reinforces existing conservative dispatch guidance. |

## Notebook promotions

- `container-api-conventions.md`: unordered/hash traversal must be explicitly sorted when it feeds deterministic user-visible or regression output (!25598).
- `dissector-context-flow-conventions.md`: do not use first-pass/`visited` guards around visible protocol-tree diagnostics that must survive redissection (!25595).
- `allocator-scope-conventions.md`: pass short-lived borrowed data explicitly when its lifetime does not match a broader context object; !25593 is the preferred evolution over !25579.

Existing notebook rules were deliberately not duplicated when this batch merely supplied corroborating evidence, including `caplen` versus original length (!25600), arithmetic safety (!25576), persistent storage lifetime (!25564), write-bound capacity checks (!25584), AI disclosure spelling (!25596), and conservative dispatch (!25559).