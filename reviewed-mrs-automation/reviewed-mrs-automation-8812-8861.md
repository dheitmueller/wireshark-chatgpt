# Wireshark MR automated review: !8861–!8812

Reviewed 2026-10-31 with GPT-6 from corpus `dheitmueller/wireshark-corpus-mrs` pinned at `ddcaa22b51c68f594e425a23388c3a2086813054`.
Notebook base commit: `257184d33f630aad3c6d362ebe5e9b3a00d6a6b2`.

**Exact reviewed membership:** !8861, !8860, !8859, !8858, !8857, !8856, !8855, !8854, !8853, !8852, !8851, !8850, !8849, !8848, !8847, !8846, !8845, !8844, !8843, !8842, !8841, !8840, !8839, !8838, !8837, !8836, !8835, !8834, !8833, !8832, !8831, !8830, !8829, !8828, !8827, !8826, !8825, !8824, !8823, !8822, !8821, !8820, !8819, !8818, !8817, !8816, !8815, !8814, !8813, !8812.

**Count/outcomes:** 50 distinct previously unreviewed MRs, descending. 49 merged; 1 closed/unmerged (!8830, superseded by !8828). All 50 records' diffs and stored discussions were examined, with deeper focus on maintainer-reviewed architectural work.

## Selection and preservation

Read all **369** preexisting ledger files in `reviewed-mrs-automation/` (including special backfill, gap, noncontiguous and exact companion records), and `reviewed-mrs.md`. No prior review ledger identifies !8861–!8812 as reviewed. The previous ledger mentions !8861 only as the *next candidate*, not a completed review. Existing previous-run frontier reconciliation, actual record memberships and the individually verified corpus records informed selection, not assumptions about numeric coverage from filenames. The pinned corpus is unchanged; there were no newly added higher candidates. The exact old !17571–!17620 ledger remains unmodified and counted: 50 unique table rows, original blob `9393377c3adeb3a38d502532839765e3abe771fe`.

## Durable findings and weighting

Merged implementation and substantive feedback of Guy Harris, John Thacker, Gerald Combs, Alexis La Goutte, Martin Mathieson and Tomasz Moń were weighted most strongly. The highest-value lessons: separating `fragment_head` and `fragment_item` requires whole-consumer repair (!8836/!8844/!8846/!8859); nested PPP layer identity must use the actual returned reassembly TVB (!8847); Qt tree caches must be scoped to the displayed packet, as Guy explicitly stressed, and UI regressions remain possible after merge (!8837); BSON recursion checks must exclude early exits and restore depth (!8828); RDP/TLS protocol negotiation must not retroactively change earlier frames on redissection (!8820); UTF-8 custom column presentation must be bounded separately from filter values (!8826); all PTP period reciprocals must be guarded (!8843). !8852 gives concrete differential tshark output testing; !8831 shows the need for representative newly introduced dissector-field paths and old compiler checks.

**Qualified/negative evidence:** Closed !8830 is not an accepted Mongo implementation. Merged !8858's Coverity guard does not convincingly restore last-fragment traversal semantics. Merged !8837 subsequently attracted an Expert Information UI crash report (#18662), so it is not an all-paths-success precedent. !8820's displayed full sequence is visibly narrowed from 64 to 16 bits in the historical diff; retain the frame-aware conversation lesson, not the truncating field usage. Backports and generated data changes were not multiplied into separate independent precedents.

## Exact MR assessments

| MR | Outcome | Depth | Assessment |
| --- | --- | --- | --- |
| !8861 | merged | scanned | Release-3.6 raises preallocated header fields to 240000; required by PCAP ASN.1 expansion. |
| !8860 | merged | scanned | Include QVector explicitly for Qt5/OpenSUSE; transitive includes are not portable. |
| !8859 | merged | deep | Reassembly follow-up fixes empty-list handling in MERGE_FRAG and fragment_add_seq_single_move. |
| !8858 | merged | deep | OPC UA Coverity guard is not proof of correct last-item traversal; loop appears to end at NULL. |
| !8857 | merged | scanned | CI schedules documented, including weekly and twice-daily rules. |
| !8856 | merged | discussion | ORAN modulation-compression bitfield/derived scaler expansion; protocol-specific, little human review. |
| !8855 | merged | discussion | PCAP v17 release-3.6 backport; Alexis required HF preallocation increase after CI failure. |
| !8854 | merged | scanned | PCAP v17 ASN.1 release-4.0 backport of !8850. |
| !8853 | merged | deep | CMake warning selection is contributor policy, with opt-in TODO warning classes. |
| !8852 | merged | deep | PCEP table-based object dispatch refactor; differential tshark -nVr output tested on seven issue captures. |
| !8851 | merged | scanned | RTPProxy Info column formats whitespace once; avoids embedded newline artifacts. |
| !8850 | merged | discussion | Master PCAP ASN.1 v17 updates source/generator and generated dissector. |
| !8849 | merged | scanned | GSM A-bis OML NACK causes become filterable uint8 with value string labels. |
| !8848 | merged | scanned | RTPProxy exposes decoded tag/media ID in Info column; capture supplied. |
| !8847 | merged | deep | PPP reassembly must use returned TVB, not reassembled frame number alone, for multilayer packets. |
| !8846 | merged | deep | Repairs empty-list/head handling in fragment_add_seq_single after !8836 refactor. |
| !8845 | merged | discussion | Qt recursive tree search forwards gpointer itself rather than address of pointer. |
| !8844 | merged | deep | OBEX and SMB pipe fragment traversal fixes after fragment-head/item separation; addresses analyzer reports. |
| !8843 | merged | deep | PTPv2 validates positive periods/intervals across divisions; Gerald identified missed sites and debug printf. |
| !8842 | merged | scanned | Mongo BSON recursion-depth release-3.6 backport of !8828. |
| !8841 | merged | scanned | Mongo BSON recursion-depth release-4.0 backport of !8828. |
| !8840 | merged | scanned | Automatic vendor, services, ASTERIX, TLS cert log and translation update master. |
| !8839 | merged | scanned | Reduced automatic data refresh on release-4.0. |
| !8838 | merged | scanned | Reduced automatic data refresh on release-3.6. |
| !8837 | merged | deep | Qt ProtoTreeModel O(1) indexed per-displayed-packet cache; Guy challenged memory lifetime; postmerge issue #18662. |
| !8836 | merged | deep | Separates fragment_head from fragment_item; downstream misuse repaired by !8844/!8846/!8859. |
| !8835 | merged | scanned | NFSv4 directory entries decode UTF-8 strings with tvb_get_string_enc. |
| !8834 | merged | scanned | SAP HDB typo and cppcheck cleanup; corroborative. |
| !8833 | merged | scanned | PostgreSQL type byte sanitized before appending to Info column. |
| !8832 | merged | discussion | New DECT DLC dissector, explicit prototypes and release notes after Alexis review. |
| !8831 | merged | deep | New DECT NWK: Alexis/Martin warn on typed-bit APIs, Clang/MSVC and later older GCC, prototype, and release notes; later fixes. |
| !8830 | closed | discussion | Closed duplicate Mongo depth proposal, superseded by merged !8828. |
| !8829 | merged | discussion | GCC release CI Werror; fuzz and Valgrind jobs Werror-off to execute diagnostics. |
| !8828 | merged | deep | Mongo depth counts actual nested docs, restores depth, tests prior denial-of-service capture; John/Gerald/Alexis. |
| !8827 | merged | discussion | TCP ACK-unseen sequence frontier uses modular LT_SEQ and correct direction; John review. |
| !8826 | merged | deep | Custom columns separate resolved/unresolved buffers, delimiters, NULL fallback and UTF-8-safe truncation. |
| !8825 | merged | scanned | WASSP NULL-safe value-string fallback backport release-3.6. |
| !8824 | merged | scanned | WASSP NULL-safe value-string fallback backport release-4.0. |
| !8823 | merged | discussion | WASSP column uses val_to_str_const fallback instead of possibly NULL try_val_to_str. |
| !8822 | merged | scanned | Protobuf lexer quote syntax release-3.6 backport. |
| !8821 | merged | scanned | Protobuf lexer quote syntax release-4.0 backport. |
| !8820 | merged | deep | RDPUDP TLS reassembly, saved extended sequence state and frame-specific TLS conversation transition. |
| !8819 | merged | scanned | Protobuf lexer supports opposite quote characters inside quoted literals; sample proto supplied. |
| !8818 | merged | scanned | EVS Info column ellipsis and pluralization. |
| !8817 | merged | scanned | EVS header-full-only preference release-4.0 backport. |
| !8816 | merged | scanned | EVS header-full-only preference when compact inference is ambiguous. |
| !8815 | merged | scanned | DNS EDNS report-channel option 18, filterable agent-domain string. |
| !8814 | merged | scanned | pcapng debug shows decimal and hex unknown option. |
| !8813 | merged | discussion | Qt Endpoint GeoIP lat/lon rendered localized with underlying typed numeric model role. |
| !8812 | merged | scanned | 802.11 OSU NAI textual field changes FT_BYTES/ENC_NA to FT_STRING/ENC_ASCII. |

## Next run

Next expected candidate is **!8811**, confirmed present in the corpus. Rebuild membership from all tracking before selecting. No builds, capture executions, fuzzing or runtime tests were performed in this documentation review.
