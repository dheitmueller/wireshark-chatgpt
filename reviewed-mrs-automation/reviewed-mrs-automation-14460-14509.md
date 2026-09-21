# Automated MR review ledger: !14460-!14509

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `1c7ad509887ee25079a7865cc62ba18cba06f49a`
Notebook starting commit: `bdec0981dbfee5e39619842ef54995243e2e1b88`
Review direction: descending from the highest-numbered previously unreviewed MR.

## Selection and deduplication

Before selecting this batch, the reviewed set was reconciled against `reviewed-mrs.md`, the available `reviewed-mrs-automation/` ledger inventory, and the immediately preceding exact ledger `reviewed-mrs-automation-14510-14559.md`. The notebook head and corpus commit were unchanged since that exact ledger was created, so its full-set reconciliation remains the current reviewed-set snapshot. The historical !17571-!17620 batch remains explicitly included, as do the later high-numbered automation ledgers (including the mixed !26567-!26577 / !14710-!14748 run). Selection was performed by MR-number membership, not by assuming numeric ranges were wholly reviewed.

At this corpus snapshot, the fifty highest-numbered MRs not in the reviewed set are exactly !14509 through !14460. This run reviews all fifty and no others.

Outcome: 49 merged MRs and one closed/unmerged draft (!14467). The closed draft is down-weighted; its recursion-suppression work is superseded by merged !14509 and related merged recursion/tooling changes.

## Exact MRs reviewed

| MR | State / target | Review | Notes |
|---|---|---|---|
| !14509 | merged, master | Scanned | Gerald Combs-authored recursion/Clang-Tidy suppression cleanup across dissectors. Corroborates existing bounded-recursion/static-analysis guidance; no new rule promoted. |
| !14508 | merged, master | Scanned | MONGO recursion guard. Part of the broad recursion-hardening series already represented by stronger notebook evidence. |
| !14507 | merged, master | Deep | John Thacker fixes GTP fields whose one-octet encodings are scaled before being added to the tree. `FT_UINT8` made valid semantic values such as 1500 impossible to filter. Promoted: register the field for the post-transform value range, not merely wire width. |
| !14506 | merged, master | Deep | John Thacker fixes display filters for `BASE_VAL64_STRING | BASE_EXT_STRING`, including symbolic-to-numeric semantic checking and numeric-to-symbolic lookup. Promoted composable display-flag handling and real-field regression testing. |
| !14505 | merged, master | Scanned | SMB2 reserved-field/update dissection work; protocol-local feature change, no additional durable convention. |
| !14504 | merged, release-4.0 | Corroborating | Guy Harris-authored stable backport of the text2pcap `-P` behavior fix. Reinforces that documented CLI semantics are part of the behavior contract. |
| !14503 | merged, release-4.2 | Corroborating | Guy Harris-authored stable backport of the text2pcap `-P` behavior fix. |
| !14502 | merged, master | Deep/corroborating | John Thacker master fix, approved by Guy Harris: `-P` now actually selects `WTAP_ENCAP_WIRESHARK_UPPER_PDU` as the man page/help already promised. Strong documentation/implementation consistency evidence; not promoted as a separate narrow rule. |
| !14501 | merged, master | Deep/corroborating | TPNCP adds bounds checks both while loading nondefault `tpncp.dat` and when malformed traffic indexes fixed arrays. Reinforces validating externally derived indexes at both configuration-ingest and packet-use boundaries. |
| !14500 | merged, master | Scanned | 5co-rap Clang-Tidy suppression cleanup; no separate durable rule. |
| !14499 | merged, release-3.6 | Scanned | RBM recursion-check backport; corroborates existing recursion guidance. |
| !14498 | merged, release-4.0 | Scanned | RBM recursion-check backport. |
| !14497 | merged, release-4.2 | Scanned | RBM recursion-check backport. |
| !14496 | merged, release-4.0 | Scanned | JPEG recursion-check backport. |
| !14495 | merged, release-4.2 | Scanned | JPEG recursion-check backport. |
| !14494 | merged, master | Scanned | RBM recursion check on master; existing recursion convention. |
| !14493 | merged, master | Scanned | Stats-tree plugin updates for the new path separator; compatibility follow-up to an already-established API change. |
| !14492 | merged, master | Scanned | BLF build fix; no reusable convention beyond keeping hardening changes build-clean. |
| !14491 | merged, master | Scanned | MP4 Clang-Tidy suppressions; tooling maintenance. |
| !14490 | merged, release-3.6 | Scanned | BLF recursion-check backport. |
| !14489 | merged, release-4.0 | Scanned | BLF recursion-check backport. |
| !14488 | merged, release-4.2 | Scanned | BLF recursion-check backport. |
| !14487 | merged, master | Scanned | JPEG recursion check on master; existing recursion convention. |
| !14486 | merged, master | Corroborating | Martin Mathieson fixes ISIS-LSP child/root bitmask mismatch; explicitly tested with generated data and fields in a test dissector, then approved by Gerald Combs. Reinforces checker-driven bitmask validation. |
| !14485 | merged, master | Scanned | BLF recursion check on master; existing recursion convention. |
| !14484 | merged, master | Scanned | SNMP RFC 5343 local-engine identification support; protocol feature with no broader review lesson. |
| !14483 | merged, master | Scanned | GNSS UBX-RXM-MEASX dissector addition; protocol-local feature change. |
| !14482 | merged, release-3.6 | Scanned | ISIS-LSP recursion-check backport. |
| !14481 | merged, release-4.0 | Scanned | ISIS-LSP recursion-check backport. |
| !14480 | merged, release-4.2 | Scanned | ISIS-LSP recursion-check backport. |
| !14479 | merged, master | Corroborating | Gerald Combs moves broad transitional Clang-Tidy exclusions to directory-level `.clang-tidy` files instead of scattering file-local recursion suppressions. Useful historical tooling evidence, but explicitly transitional ('we don't want to do any checks in this directory yet'), so not promoted as a general policy. |
| !14478 | merged, master | Corroborating | Martin Mathieson fixes PLDM version-string buffer sizing and passes remaining capacity rather than total capacity to subsequent formatting calls. Coverity-driven robustness; existing buffer-bound rules already cover it. |
| !14477 | merged, master | Scanned | ISIS-LSP recursion check on master. |
| !14476 | merged, release-4.2 | Scanned | GIOP recursion-check backport. |
| !14475 | merged, release-4.2 | Scanned | ENRP recursion-check backport. |
| !14474 | merged, master | Scanned | GIOP recursion check on master. |
| !14473 | merged, master | Scanned | ENRP recursion check on master. |
| !14472 | merged, master | Scanned | VP9 tree labels annotated with the short names used by filters; improves discoverability but does not establish a broader new convention. |
| !14471 | merged, master | Deep | John Thacker adds `test_for_regular_file()` and uses it when copying profile configuration so directories/FIFOs/devices are not treated as ordinary preference files. Promoted filesystem-object-type validation for config I/O. |
| !14470 | merged, master | Deep | John Thacker makes extcap preference-file failure independent from writing the main preferences file and adds clean fallback when a module path is a directory. Promoted auxiliary/primary configuration failure isolation. |
| !14469 | merged, master | Scanned | Couchbase documentation/link correction to memcache text protocol; no code convention. |
| !14468 | merged, master | Corroborating | X.509 organizationIdentifier corrected in the ASN.1 `.cnf` source and regenerated dissector output together. Reinforces editing generator/source-of-truth inputs and regenerating derived code. |
| !14467 | closed, master, draft | Down-weighted | Unmerged draft adding recursion suppressions. Closed by Gerald Combs and superseded by merged !14509/tooling work; not accepted project precedent. |
| !14466 | merged, master | Scanned | CBOR recursion-check update; existing recursion guidance. |
| !14465 | merged, master | Scanned | BACapp recursion-check update; existing recursion guidance. |
| !14464 | merged, master | Deep/corroborating | John Thacker strengthens CLTP-over-UDP recognition using legal protocol parameters, rejects zero/reserved length values, and moves the heuristic to the OSI transport owner. Reinforces existing cheap-protocol-invariant and ownership guidance rather than duplicating it. |
| !14463 | merged, master | Deep | LTP cancel/cancel-ack correlation plus heuristic. Anders Broman flags the heuristic as potentially CPU-heavy and asks that it be disabled by default; author agrees/changes it before merge. Promoted performance-aware heuristic default policy. |
| !14462 | merged, master | Scanned | DNSCrypt initial-DNS-request support with sample capture and naming/check-script follow-up. Useful submission/test evidence, no distinct new rule. |
| !14461 | merged, master | Scanned | Adds missing Roon Discovery `query_service_id` field; straightforward protocol completeness fix. |
| !14460 | merged, master | Corroborating | Adds raw Thrift sub-dissector API. Gerald Combs points out new Clang-Tidy recursion findings outside the existing nested-depth checks, reinforcing static analysis as a backstop for hand-maintained recursion guards; later recursion work is stronger precedent. |

## Durable notebook changes promoted from this batch

- `field-value-semantics-conventions.md`: the registered field type must cover the semantic/post-transform value actually added to the protocol tree, not merely the width of the wire encoding. Primary evidence: merged !14507.
- `dfilter-conventions.md`: legal `hfinfo->display` traits such as 64-bit values and extended value-string tables must be handled compositionally across both semantic checking and execution/formatting. Primary evidence: merged !14506.
- `configuration-file-io-conventions.md` (new): verify the expected filesystem object type before copying/reading a configuration file, and isolate auxiliary preference-file failures from the primary preferences write. Primary evidence: merged !14471 and !14470, both authored and merged by John Thacker.
- `heuristic-dissector-conventions.md`: default heuristic enablement must account for no-match execution cost and invocation breadth in addition to false-positive selectivity. Primary evidence: merged !14463 with direct Anders Broman review and author follow-up.

## Strong corroborating evidence retained without duplicate notebook rules

- !14502 plus Guy Harris-authored stable backports !14503/!14504 show that help/man-page claims about a CLI option are behavioral contracts, not aspirational documentation.
- !14501 validates bounds both when external configuration populates a fixed database and when packet data later indexes it; this is a useful two-boundary robustness exemplar.
- !14464 independently demonstrates strengthening heuristics with cheap normative structure before claiming UDP traffic; existing heuristic/protocol-validation notes already capture that rule.
- !14468 changes the ASN.1 configuration source and generated C together, reinforcing the existing generated-source-of-truth discipline.
- The large Gerald Combs recursion series and the !14460 Clang-Tidy observation reinforce existing recursion and checker guidance. !14467 is specifically not promoted because it is a closed/unmerged draft superseded by accepted work.

## Frontier after this run

`mr_14459.json` exists in the same corpus commit and is merged. Therefore the corpus is not exhausted. If no newer previously unreviewed MR is added before the next run, !14459 is the next descending candidate.
