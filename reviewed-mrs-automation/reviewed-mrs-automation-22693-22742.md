# Wireshark MR review automation ledger: !22693-!22742

- Corpus: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`
- Selection method: rebuilt the already-reviewed set from `reviewed-mrs.md` plus all available files under `reviewed-mrs-automation/`; selected the 50 highest-numbered corpus MRs not in that set.
- Historical tracking: the previously reviewed !17571-!17620 batch remains preserved and counted as already reviewed.
- Exact batch size: **50**.
- Direction: newest to oldest.

| MR | Depth | Review result |
|---|---|---|
| !22742 | Deep | Merged NATS INFO-column operation names. John Thacker suggested a case-insensitive string dissector table/hash lookup if command count/performance warrants it; useful future optimization, not promoted as a current requirement. |
| !22741 | Deep | Merged John Thacker conversion of `tvb_new_subset*` offsets/lengths to unsigned domains; explicitly notes third-party callers using `-1` should migrate to `tvb_new_subset_remaining()`. Corroborates existing TVBuff API-domain guidance. |
| !22740 | Scanned | Merged `check_typed_item_calls` parallelization with `concurrent.futures`; tooling implementation improvement, no additional durable convention. |
| !22739 | Deep | Merged John Thacker cleanup of incorrect `-1` offsets. Reinforces that negative offsets on subset TVBuffs do not mean “parent byte” and whole-buffer/generated items should be expressed explicitly. Covered by existing TVBuff guidance. |
| !22738 | Scanned | Merged John Thacker LBMR negative-offset removal. Corroborates the unsigned-offset migration. |
| !22737 | Scanned | Merged unsigned offsets for `tvb_*_length_remaining`. Corroborates existing unsigned-domain guidance. |
| !22736 | Scanned | Merged WiMAX ASNCP use of `tvb_new_subset_length`; prefers the helper that derives captured length correctly. Corroborates subset-TVBuff guidance. |
| !22735 | Scanned | Merged `jtckdint` workaround for C `_Generic` lvalue conversion. Portability/toolchain detail; no broader new rule beyond existing C type guidance. |
| !22734 | Scanned | Merged addition of `jtckdint.h` to `wireshark.h` so checked integer arithmetic is broadly available. Supports existing checked-arithmetic guidance. |
| !22733 | Scanned | Merged Stratoshark inheritance of project patch/build/version-extension values. Product-version consistency change; no separate convention added. |
| !22732 | Deep | Merged initial AI-usage disclosure/dev-hook framework. Historically important, but its `AI-Assisted` spelling was later superseded by `Assisted-by`; existing submission guidance from !26304 is the authoritative durable rule. |
| !22731 | Deep | Closed/unmerged draft release-4.4 backport of the IEEE 802.11 A-MSDU Mesh Control fix; failed pipeline and was closed. Down-weighted relative to merged !22730/!22721. |
| !22730 | Deep | Merged release-4.6 backport of IEEE 802.11 Mesh Control placement. Review exposed an explicit dependency on !22734 (`ckd_sub` availability), reinforcing dependency-aware backports; no new standalone rule. |
| !22729 | Deep | Merged generated IANA address-family/IP-protocol data. Accepted architecture keeps a dedicated public shared `packet-iana-data.h`; Guy Harris explicitly checked third-party/public API implications. Promoted shared registry-data ownership rule. |
| !22728 | Deep | Merged Gerald Combs addition of `system()` to prohibited APIs, approved/merged by Michael Mann. Strong accepted safety/tooling signal, but the MR provides little rationale/approved replacement, so recorded without overgeneralizing a new process-execution rule. |
| !22727 | Scanned | Merged `make-enums.py` path cleanup so it runs from the tools directory. Tool usability improvement, no durable architecture lesson. |
| !22726 | Scanned | Merged macOS third-party-download CI caching. Build-performance optimization, no durable correctness convention. |
| !22725 | Scanned | Merged Process Monitor configuration profile. Feature/UI configuration addition, no reusable review rule identified. |
| !22724 | Scanned | Merged SRP use of common checksum helper plus removal of a negative TVBuff offset. Reinforces common-helper and unsigned-offset guidance. |
| !22723 | Scanned | Merged `_FORTIFY_SOURCE` override handling in CMake. Build configurability change; later sanitizer-specific guidance is stronger and already recorded. |
| !22722 | Scanned | Merged `check_val_to_str.py` parallelization with futures. Tooling speed improvement only. |
| !22721 | Deep | Merged master IEEE 802.11 A-MSDU Mesh Control placement fix underlying the release backports. Correctly adjusts protocol-location semantics and checked length subtraction; no new general rule beyond existing parser/bounds guidance. |
| !22720 | Deep | Merged InfiniBand fix retaining the heuristic dissector selected before RC_SEND reassembly. Prevents SMB Direct traffic from being reclassified as RPC-over-RDMA after reassembly. Promoted heuristic/reassembly identity rule. |
| !22719 | Scanned | Merged unsigned offsets for `tvb_bytes_exist` / `_offset_exists`; corroborates existing TVBuff domain migration. |
| !22718 | Scanned | `_FORTIFY_SOURCE` override CMake work in the same family as !22723; no additional durable rule. |
| !22717 | Scanned | Merged eCPRI root-header payload-size presentation. UI/protocol presentation improvement only. |
| !22716 | Scanned | Merged NMEA dead-store removal from Clang Analyzer. Straightforward static-analysis cleanup. |
| !22715 | Scanned | Merged ACN cleanup removing redundant `pdu_len` state and satisfying Clang Analyzer. No broader rule. |
| !22714 | Deep | Merged successor to !22711 using the existing JSON dissector for NATS INFO/CONNECT JSON. Reinforces reuse of an established structured parser rather than duplicating parsing; already covered by parser-reuse conventions. |
| !22713 | Scanned | Merged TCNCP preference for database-file path. User-configurability improvement, no durable architecture rule extracted. |
| !22712 | Scanned | Merged IEEE 802.11 dead-store cleanup from Clang Analyzer. Straightforward analyzer fix. |
| !22711 | Deep | Closed/unmerged first NATS JSON-dissector submission after a failed pipeline; author explicitly recreated it as !22714. Down-weighted as superseded, while preserving its review history. |
| !22710 | Scanned | Merged IPSec null-heuristic bounds fix for captures shorter than reported length. Corroborates existing captured-vs-reported and unsigned-offset guidance. |
| !22709 | Scanned | Merged ITS/asn2wrs removal of obsolete `OMIT_ASSIGNMENT`. Generated-dissector maintenance cleanup. |
| !22708 | Scanned | Merged explicit CMake PKCS support option. Build-feature configurability change, no additional general rule. |
| !22707 | Scanned | Merged CBOR undefined-behavior fix; checking after signed overflow is too late because the expression itself is UB. Corroborates arithmetic-safety guidance. |
| !22706 | Scanned | Merged IUA UB fix documenting integer promotions of `uint16_t` to signed `int`. Directly corroborates the promoted-expression-type rule already captured from !22808. |
| !22705 | Scanned | Merged TPNCP database-path preference. Configuration flexibility change, no broader lesson. |
| !22704 | Scanned | Merged unsigned offsets for `tvb_ensure_bytes_exist`; corroborates TVBuff API-domain guidance. |
| !22703 | Scanned | Merged P1 ASN.1 classification correction eliminating an asn2wrs warning. Focused generated-dissector input fix. |
| !22702 | Scanned | Merged common username column and `pinfo` field for Falco/Procmon. Shared metadata feature; no additional durable convention. |
| !22701 | Scanned | Merged MsgPack Protocol/Info column completion. Presentation fix only. |
| !22700 | Scanned | Merged `check_tfs.py` parallelization via `concurrent.futures`. Tool speed improvement only. |
| !22699 | Scanned | Merged asn2wrs quiet-mode handling for identical duplicate directives. Tool diagnostic-noise refinement. |
| !22698 | Deep | Closed/unmerged draft moving shared AFNUM data into BGP. João Valverde strongly rejected arbitrary consumer ownership/public-API regression; superseded by the accepted !22729 design. Used as negative evidence for the promoted shared-registry-data rule. |
| !22697 | Scanned | Merged removal of a decades-obsolete PKIX workaround. Legacy cleanup; no reusable rule beyond verifying whether compatibility hacks are still necessary. |
| !22696 | Scanned | Merged typo fixes, mostly non-user-visible. No durable lesson. |
| !22695 | Scanned | Merged missing ASN.1 `EXPORT_DEPENDS` declarations to make fresh generation warning-free. Reinforces keeping generator dependency metadata complete. |
| !22694 | Deep | Merged John Thacker CI regeneration of CORBA IDL dissectors, approved/merged by Anders Broman. Explicitly verifies generated code has not been edited directly. Promoted generated-code CI invariant. |
| !22693 | Scanned | Merged VoIP/Q.931 completion-status lookup optimized to traverse newest calls first. Local data-structure/behavior fix, no broader convention extracted. |

## Promoted durable findings

1. **Heuristic selection is reassembly state** — !22720: preserve the chosen heuristic dissector across fragment reassembly instead of rerunning competing heuristics on the completed unit.
2. **Generated output must be reproducible in CI** — !22694: regenerate committed generated dissectors and reject drift/hand edits.
3. **Shared public protocol registry data needs shared ownership** — rejected !22698 plus merged !22729: do not bury shared constants/value strings in an arbitrary consumer dissector; preserve a deliberate public shared API for unrelated dissectors and plugins.

## Strong corroboration not duplicated in notebook rules

The batch repeatedly reinforces existing guidance for unsigned TVBuff offset/length domains (!22741, !22739, !22738, !22737, !22736, !22724, !22719, !22710, !22704), checked arithmetic and C integer-promotion semantics (!22734, !22707, !22706), reuse of specialized parsers (!22714), and synchronization of contribution-policy enforcement (!22732, whose original trailer spelling was later superseded).
