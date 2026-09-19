# Wireshark MR review automation ledger: !10250-!10299

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit reviewed: `08420a2f7851841b7a38714f642987c8cd167184`
- Direction: descending MR number, newest available unreviewed MRs toward older MRs.
- Selection rule: fifty highest-numbered corpus MRs not present in the accumulated review tracking set.
- Previously reviewed !17571-!17620 remains part of the reviewed set and was preserved when selecting this batch.

## Exact reviewed MR set

!10299, !10298, !10297, !10296, !10295, !10294, !10293, !10292, !10291, !10290,
!10289, !10288, !10287, !10286, !10285, !10284, !10283, !10282, !10281, !10280,
!10279, !10278, !10277, !10276, !10275, !10274, !10273, !10272, !10271, !10270,
!10269, !10268, !10267, !10266, !10265, !10264, !10263, !10262, !10261, !10260,
!10259, !10258, !10257, !10256, !10255, !10254, !10253, !10252, !10251, !10250.

Count: **50**.

## Durable findings

- **!10287 — Deep, merged.** `ATapDataModel` is a flat table even though it is displayed through `QTreeView`. Its `rowCount()` therefore must return zero for a valid parent index. Qt itself can exercise that path even when Wireshark does not call it directly. Added `qt-model-conventions.md` to record the framework-contract lesson. Authored by John Thacker; approved and merged by Gerald Combs.
- **!10299 — Deep, merged.** DRDA SQLSTT parsing was expanded to model the protocol's mutually exclusive string encodings and later nullable-CLOB representation rather than treating the SQL statement as a single simplistic string. Useful protocol-specific parser evidence; no separate general rule added.
- **!10275 — Deep, merged release backport.** RTP timestamp rollover arithmetic must measure each side of the wrap in the correct direction; this backport fixes an inverted difference calculation. Useful corroboration for explicit wraparound arithmetic, but existing arithmetic/sequence guidance already covers the general lesson.
- **!10263 — Scanned/merged.** DNS IPSECKEY algorithm registry extended for ECDSA and EdDSA according to the relevant RFCs. Straightforward standards-table maintenance; no new durable convention.
- **!10254 — Scanned/merged.** `check_type_item_calls.py` work begins substituting known macros while analyzing calls, reinforcing the project's preference for semantic static checks over purely textual matching. Existing static-analysis guidance is sufficient.
- **!10276 — Closed/unmerged.** A `-Wnonnull` cleanup passed CI but was closed as stuck in GitLab limbo. It was weighted below merged evidence and not used as an accepted implementation exemplar.

The remaining MRs in the exact set were reviewed at scan depth unless their discussion/diff justified deeper treatment; no additional durable convention was strong enough to warrant notebook duplication in this run.

## Continuation

Rebuild the complete reviewed set from all per-run ledgers plus `reviewed-mrs.md` before the next selection. Because the scraper has a large gap between the prior reviewed !15935 boundary and this newly supplied !10299 batch, do not infer that !10300-!15934 are reviewed; select only from files actually present in the corpus. Subject to the complete-set check, the next descending candidate from this corpus batch is !10249 if/when it is present.
