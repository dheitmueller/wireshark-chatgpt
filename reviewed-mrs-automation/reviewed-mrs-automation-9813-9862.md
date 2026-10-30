# Wireshark MR review ledger — !9862 through !9813

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`
Notebook starting commit: `d4a9f78097efd4ba4dc4cc1846baa27ab8d72e34`
Review date: 2026-10-30
Review direction: highest-numbered unreviewed to older.
**Exactly 50 distinct merge requests: 48 merged; two closed/unmerged (!9859 and !9852).**

## Selection and duplication audit

Consulted `reviewed-mrs.md`, the supplemental `reviewed-mrs-automation.md`, all 332 ordinary per-run files, and the 17 exceptional/noncontiguous/mixed/gap/exact-list tracking files in `reviewed-mrs-automation/`. Explicit reviewed-membership records were used rather than assuming every MR between filename endpoints was reviewed. Special cases include the empty, unreviewable `mr_15376.json`; mixed new high-numbered records !26578-!26580; and the two earlier noncontiguous near-frontier batches. The preceding exact ledger covers !9912-!9863, and the next fifty available unreviewed corpus records are precisely !9862-!9813.

The historical `reviewed-mrs-automation-17571-17620.md` was independently checked: **all 50 distinct entries, !17571 through !17620, remain present and were excluded**. Its original file was not modified.

Each selected MR's description, final code diff, merge outcome, and substantive discussion were examined from the corpus. Merged master fixes and authoritative reviewer comments were weighted above release cherry-picks, closed proposals, and intermediate implementation steps. In particular, Guy Harris-authored master !9846 provides the authoritative Gryphon field semantics; !9859 and !9852 were closed/superseded. The merged BBLog frame-centric !9838 was only a historical stage: later merged !19957/!19980 and Stig Bjørlykke's review establish registered extension handling as the preferred architectural direction.

## Exact reviewed MR numbers (descending)

!9862, !9861, !9860, !9859, !9858, !9857, !9856, !9855, !9854, !9853, !9852, !9851, !9850, !9849, !9848, !9847, !9846, !9845, !9844, !9843, !9842, !9841, !9840, !9839, !9838, !9837, !9836, !9835, !9834, !9833, !9832, !9831, !9830, !9829, !9828, !9827, !9826, !9825, !9824, !9823, !9822, !9821, !9820, !9819, !9818, !9817, !9816, !9815, !9814, !9813

## Individual review findings

| MR | Outcome | Finding |
| --- | --- | --- |
| !9862 | merged | Guy Harris release-3.6 Gryphon IOCTL backport: zero response-byte span and FT_UINT32. |
| !9861 | merged | Release-4.0 same Guy Harris IOCTL correction; corroboration. |
| !9860 | merged | Release-3.6 field/value_string width audit, superseding !9859. |
| !9859 | closed/unmerged | Closed failed GitLab merge/rebase attempt, retried as merged !9860. |
| !9858 | merged | Release-4.0 backport of master field-width audit !9837. |
| !9857 | merged | Locale-aware Decode As sorting, not QMap case-sensitive ordering. |
| !9856 | merged | Replace ambient packet allocator lookup with pinfo->pool or explicit scope parameter. |
| !9855 | merged | Logging guide grammar corrections following !9831. |
| !9854 | merged | Accepted RTPS topic-query and DATA_SESSION field additions. |
| !9853 | merged | Correct Time Display Format tooltip semantics. |
| !9852 | closed/unmerged | Closed duplicate RTPS proposal superseded by merged !9854. |
| !9851 | merged | BBLog adds FreeBSD PRU event decoding. |
| !9850 | merged | Named TLS and QUIC GREASE predicate macros; later safety refinements already documented. |
| !9849 | merged | Mark longjmp-sensitive pcapng handler pointer volatile; fix -Wclobbered. |
| !9848 | merged | 3GPP Diameter AVPs updated in dissector and XML registry. |
| !9847 | merged | BBLog adds new TCP event identifiers. |
| !9846 | merged | Guy Harris master Gryphon response field: value from request, no fake bytes, FT_UINT32. |
| !9845 | merged | Guy Harris explains intentional GMR-1 ambiguous IDs and synthetic +0x100 aliases. |
| !9844 | merged | Accepted Qt5/6/C++11 range-for fix supersedes earlier !9822/!9836 errors. |
| !9843 | merged | BBLog timer events and event subtype decoding. |
| !9842 | merged | Add RSVP S2L_SUB_LSP field registration; restores JSON/filter identity. |
| !9841 | merged | BBLog event-specific Info column and unknown-event fallback. |
| !9840 | merged | John Thacker/Roland Knall: stable proxy/source filter-column mapping, avoid hot path. |
| !9839 | merged | TRDP new dissector with sample pcap and release note; TCP framing needs tcp_dissect_pdus(). |
| !9838 | merged | Early BBLog code in packet-frame criticized by Stig Bjørlykke; later extension API supersedes design. |
| !9837 | merged | Master field widths versus value_string tables audit, templates and generated C. |
| !9836 | merged | Intermediate Qt C++11 change broken on Qt5 index types, corrected by !9844. |
| !9835 | merged | Master generated weekly registries; routine. |
| !9834 | merged | Release-3.6 generated weekly registries; routine. |
| !9833 | merged | Release-4.0 generated weekly registries; routine. |
| !9832 | merged | BBLog FreeBSD errno names added for registered numeric field. |
| !9831 | merged | WSDG logging guide, detailed João Valverde/Jaap Keuter review. |
| !9830 | merged | RTPS fuzz OOM fix: incremental wmem array, finalize to trimmed storage. |
| !9829 | merged | BBLog TCP state labels aligned to RFC 9293. |
| !9828 | merged | RTPS registered timestamp-present flag restored. |
| !9827 | merged | Bluetooth LMP NAME_RES offset/length order corrected against spec. |
| !9826 | merged | Filter-field interest queries permit targeted colorization updates. |
| !9825 | merged | John Thacker fixes Qt model/proxy ownership leaks with parentage. |
| !9824 | merged | sshdump doas support, old CLI alias and persisted preference migration. |
| !9823 | merged | Reject Lua TvbRange len < -1 sentinel, report Lua error. |
| !9822 | merged | Packet comments move into Qt model; introduced C++17 regression fixed by !9844. |
| !9821 | merged | O-RAN CUS section extension type 13 and LAA decoding. |
| !9820 | merged | Statistics guide stray word removed. |
| !9819 | merged | Timeline delegate fixes source/view column transformations; Roland Knall review. |
| !9818 | merged | TCP reassembly pending flag retained until fragment progress. |
| !9817 | merged | MySQL LOCAL INFILE data source; John Thacker flags byte-only state ambiguity. |
| !9816 | merged | O-RAN CUS filter and compression value corrections. |
| !9815 | merged | O-RAN CUS section extension 19 support. |
| !9814 | merged | SIP probable one-byte TCP keepalive, release-3.6 backport. |
| !9813 | merged | SIP probable one-byte TCP keepalive, release-4.0 backport. |

## Notebook updates

- `input-resource-limit-conventions.md`: durable guidance extracted and anchored to accepted MRs.
- `wmem-container-lifetime-conventions.md`: durable guidance extracted and anchored to accepted MRs.
- `protocol-field-semantic-conventions.md`: durable guidance extracted and anchored to accepted MRs.
- `qt-model-conventions.md`: durable guidance extracted and anchored to accepted MRs.
- `build-language-standard-conventions.md`: durable guidance extracted and anchored to accepted MRs.
- `qt-object-parenting-conventions.md`: durable guidance extracted and anchored to accepted MRs.
- `preference-migration-conventions.md`: durable guidance extracted and anchored to accepted MRs.
- `tcp-desegmentation-conventions.md`: durable guidance extracted and anchored to accepted MRs.
- `logging-conventions.md`: durable guidance extracted and anchored to accepted MRs.
- `wslua-conventions.md`: durable guidance extracted and anchored to accepted MRs.

Other findings already adequately covered in existing notebook conventions include GREASE predicate correctness (!9850 and later !11428/!11433), unusual GMR-1 numeric aliases (!9845 and earlier-reviewed !9863/!9864), new dissector sample-capture/release-note requirements (!9839), and pcapng extension architecture (!9838 and later !19957/!19980). No duplicate rule was added solely to increase the change count.

**Next expected candidate: !9812**, subject to any higher-numbered newly scraped records and explicit membership recheck.
