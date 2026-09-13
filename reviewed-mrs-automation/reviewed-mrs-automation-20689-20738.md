# Automated Wireshark MR review: !20689-!20738

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection rule: rebuilt the already-reviewed set from all available files in `reviewed-mrs-automation/` plus sparse entries in `reviewed-mrs.md`, explicitly preserving and counting the historical !17571-!17620 batch. The previous automated batch ended at !20739. None of !20738 through !20689 appeared in the tracking, and all 50 IDs exist in the pinned corpus, so this run reviewed exactly those fifty MRs.

Weighting rule: merged master MRs and accepted successors carry the greatest weight; stable backports mainly corroborate master changes; closed drafts, abandoned iterations, and changes later reverted are retained in the ledger but are not treated as accepted architectural precedent. Senior-maintainer-authored changes and substantive maintainer review are weighted especially strongly.

## Exact reviewed set

| MR | Review level | Notes |
|---|---|---|
| !20738 | Scanned | SGP.22/SGP.32 Remote SIM Provisioning dissectors; merged. No additional generic convention promoted. |
| !20737 | Scanned | ASN.1 autotag-generation fix, cherry-picked form; merged and primarily corroborating. |
| !20736 | Deep | IEEE 802.11 VHT user-position fix/refactor with supplied sample pcap and before/after validation; merged. Strong testing exemplar, already covered by capture-based validation guidance. |
| !20735 | Scanned | ASN.1 autotag-generation fix; merged. Corrects automatic-tag behavior when manual tags are present. |
| !20734 | Discussion-focused | ciscodump cleanup now exits IOS configuration mode after removing its ACL. John Thacker sought domain-owner confirmation and merged after explicit confirmation that the fix was correct. |
| !20733 | Scanned | BER/ASN.1 file parser support for larger tags; merged. |
| !20732 | Scanned | Exports PKIX SubjectKeyIdentifier for another ASN.1-based dissector; merged. |
| !20731 | Scanned | OID array pre-sizing optimization; merged. |
| !20730 | Deep | John Thacker-authored DICOM follow-up reserves exact capacity in large fixed wmem maps; merged. Promoted with !20728 to lifecycle/lookup-structure guidance. |
| !20729 | Deep | John Thacker-authored logcat-text change creates fixed GRegex objects once and destroys them at shutdown rather than capture init/cleanup; merged. Promoted to lifecycle guidance. |
| !20728 | Deep | John Thacker-authored DICOM change moves constant generated lookup maps to one-time epan-scope construction; merged. Promoted to lifecycle/lookup-structure guidance. |
| !20727 | Scanned | etwdump release-note update; merged. |
| !20726 | Scanned | BLF CAN FD burst-flag typo correction; merged. |
| !20725 | Deep | John Thacker-authored MaxMindDB startup fix defers helper launch until effective preferences are applied and handles unchanged defaults; merged. Promoted to initialization guidance. |
| !20724 | Scanned | collectd returns after decipher failure (static-analysis/Coverity fix); merged. Corroborates failure-path control-flow discipline. |
| !20723 | Scanned | `float()` display-filter function release-note entry; merged. |
| !20722 | Scanned | Makes internal ZigBee helper functions static; merged. |
| !20721 | Scanned | XML fully-qualified-name construction optimization; merged. |
| !20720 | Scanned | IEEE 802.11 vendor-specific IE parsing corrections; merged. |
| !20719 | Scanned | PFCP Juniper L2TP IE support; merged. |
| !20718 | Discussion-focused (closed/superseded) | Broader draft XML/libxml2 DTD-parser redesign. Superseded by the later accepted !20684 approach and not treated as final architecture. |
| !20717 | Scanned | Adds Zabbix test captures including legacy, TLS, and proxy/redirection cases; merged. Corroborates representative-capture testing. |
| !20716 | Deep (revert) | Gerald Combs-authored revert removes Windows-builder `check-job.ps1` after-scripts added by !20689. The reverted behavior is not retained as accepted CI precedent. |
| !20715 | Deep | XML fixed encoding regex moved from capture/preference init-cleanup lifetime to registration/shutdown lifetime; merged. Promoted to lifecycle guidance. |
| !20714 | Deep | WPS helper uses an explicit presentation flag instead of NULL `packet_info *`, preserving packet context and enabling explicit allocator scope; merged. Promoted as corroborating packet-context guidance. |
| !20713 | Scanned | Frees the epan directory queue container itself; merged. Corroborates complete ownership cleanup. |
| !20712 | Scanned | XML failure path frees copied strings that remain in a partially processed pointer array; merged. Corroborates failure-path ownership cleanup. |
| !20711 | Scanned | SOME/IP UAT validation restricts base types to supported type/length combinations and removes dead code; merged. Corroborates validating configuration at ingestion. |
| !20710 | Scanned | Dissector-warning cleanup; merged. |
| !20709 | Scanned | Stable-branch SCCP LUDT/LUDTS segmentation backport; merged. Corroborating only. |
| !20708 | Deep | collectd adds UAT-backed signature verification/decryption support; merged. Reviewed with the prerequisite parser restructuring in !20690. |
| !20707 | Scanned | Final merged MQTT-SN topic-ID/short-topic handling on master. Earlier iterations in this batch receive lower weight. |
| !20706 | Scanned (earlier iteration) | MQTT-SN topic-ID/short-topic handling iteration; lower weight than merged successor !20707. |
| !20705 | Scanned (earlier iteration) | MQTT-SN topic-ID/short-topic handling iteration; lower weight than merged successor !20707. |
| !20704 | Scanned | John Thacker-authored master SCCP LUDT/LUDTS segmentation support, based on Q.712/Q.713 despite uncommon usage; merged. |
| !20703 | Scanned (earlier iteration) | MQTT-SN topic-ID/short-topic handling iteration; lower weight than merged successor !20707. |
| !20702 | Scanned | PFCP Juniper Targeted Distribution IE support; merged. |
| !20701 | Scanned (backport) | Release-4.2 backport of wmem-map odd hash-multiplier fix; merged. |
| !20700 | Scanned (backport) | Release-4.4 backport of wmem-map odd hash-multiplier fix; merged. |
| !20699 | Scanned (earlier iteration) | MQTT-SN topic-ID/short-topic handling iteration; lower weight than merged successor !20707. |
| !20698 | Deep | John Thacker-authored master wmem-map fix forces the random multiplicative hash factor odd to avoid power-of-two collapse and severe hash-table degradation; merged. Strong specialized defensive-algorithm evidence. |
| !20697 | Scanned (closed) | Earlier MQTT-SN topic-handling submission; closed and superseded by later merged work. |
| !20696 | Scanned | Galileo OSNMA HKROOT reassembly/dissection; merged. |
| !20695 | Deep | Broad replacement of ambient `wmem_packet_scope()` uses with explicit packet context/`pinfo->pool`; merged. Strongly corroborates existing allocator-scope convention. |
| !20694 | Scanned | SOME/IP UAT-definition cleanup; merged. |
| !20693 | Scanned | WiMAX ASN CP XML loading uses `g_slist_prepend` rather than repeated append traversal; merged. Useful localized performance cleanup. |
| !20692 | Scanned | Qt plot context-menu warning fixes; merged. |
| !20691 | Scanned | O-RAN FH CUS small correctness/display improvements; merged. |
| !20690 | Deep | John Thacker-authored collectd refactor separates recursive part parsing from one-time column/tap side effects and removes semantically invalid NULL-tree shortcut; merged. Promoted to dissector pipeline guidance. |
| !20689 | Scanned (later reverted) | Added Windows-builder job-check after-scripts, but the change was subsequently reverted by !20716. Recorded for completeness, not treated as current CI precedent. |

## Durable notebook extraction

`initialization-lifecycle-conventions.md` now records three related rules from this batch: fixed resources independent of capture/preferences should use registration/epan-to-shutdown lifetime (!20715, !20729); immutable generated lookup structures should not be rebuilt per file/dissection and should reserve known capacity (!20728, !20730); and configuration-dependent side effects must wait until effective preferences are authoritative (!20725).

`dissector-pipeline-conventions.md` records the accepted collectd architecture from !20690: recursive semantic parsing should be separated from one-time packet presentation/tap side effects, and NULL-tree operation must not skip semantics that affect other outputs. It also records !20714's explicit-flag-over-NULL-pinfo pattern and the accepted !20684 XML parser migration principle: replace parsing machinery while preserving established registration/output behavior unless semantic changes are separately intended and validated.

Other strong findings principally corroborate existing notebook rules. !20695 reinforces explicit packet allocator context. !20712/!20713 reinforce complete cleanup of partially constructed data and its containers. !20717 and !20736 reinforce capture-driven validation. !20698 is a strong but specialized hash-table correctness/performance example. !20716 explicitly prevents the reverted !20689 Windows CI change from being learned as durable precedent.
