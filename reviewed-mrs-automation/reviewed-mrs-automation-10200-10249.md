# Automated MR review ledger: !10200-!10249

Reviewed 2026-09-19 from `dheitmueller/wireshark-corpus-mrs` corpus commit `08420a2f7851841b7a38714f642987c8cd167184`.

Selection method: rebuilt the already-reviewed set from the available `reviewed-mrs-automation/` ledgers plus `reviewed-mrs.md`, preserving the historical !17571-!17620 batch. The corpus currently contains a !10000-!10299 block; !10250-!10299 was already recorded by the immediately preceding run. The fifty highest-numbered corpus MRs not already in the tracking set are therefore the exact set below. No inference was made that the absent !10300-!15934 gap was reviewed.

## Exact reviewed set (50)

!10249, !10248, !10247, !10246, !10245, !10244, !10243, !10242, !10241, !10240,
!10239, !10238, !10237, !10236, !10235, !10234, !10233, !10232, !10231, !10230,
!10229, !10228, !10227, !10226, !10225, !10224, !10223, !10222, !10221, !10220,
!10219, !10218, !10217, !10216, !10215, !10214, !10213, !10212, !10211, !10210,
!10209, !10208, !10207, !10206, !10205, !10204, !10203, !10202, !10201, !10200.

## Review weighting and durable findings

Merged master changes and substantive maintainer review were weighted most heavily. Stable-branch cherry-picks were treated primarily as corroboration. Closed or still-open work was down-weighted and was not promoted as accepted architecture.

- **!10248 — Deep, merged master:** MySQL zstd compressed-traffic support. The contributor supplied a focused `mysql_zstd.pcapng` reproducer/example and described how to generate zstd traffic with the MySQL client. This reinforces the existing notebook convention that protocol behavior changes should be accompanied by concrete capture evidence when practical; no new convention text was needed.
- **!10240 — Discussion-focused, still open:** large VITA 49.2 dissector. Alexis La Goutte asked the contributor to rebase, remove pcap/PDF artifacts from the MR itself, and move an unrelated `win-setup.ps1` change to another MR. The contributor then removed the extra documentation and reverted the setup-script change. This is useful scope-discipline evidence, but the MR remains open with unresolved discussion and a failed pipeline in the corpus snapshot, so it is not treated as accepted implementation precedent.
- **!10233 — Deep, merged master, Gerald Combs-authored/merged:** extends conversation filters with caller-supplied user data and adds `CE_INT`, enabling Falco Bridge filtering without protocol-specific global coupling. Useful accepted API-extension exemplar; no distinct notebook rule beyond existing architecture/API guidance was warranted.
- **!10232 — Discussion-focused, closed unmerged:** proposed `frame_num_t` semantic typedef. Stig Bjørlykke questioned whether a typedef compatible with `guint32` could reliably enforce complete conversion and suggested compiler-enforced incompatibility. The author later closed it for lack of interest. This is useful type-safety design context, but not accepted project precedent.
- **!10229 — Deep, merged master:** new RDP RAIL channel dissector. Alexis caught four field-registration/type/length warnings (zero masks, FT_UINT8 with incompatible lengths); the author corrected them before merge. Strong corroboration that dissector-check warnings are submission blockers rather than cosmetic diagnostics; already represented in pre-submit/testing conventions.
- **!10228 — Deep, merged master:** MAPI version normalization supplied two capture files and screenshots showing behavior. Reinforces capture-backed dissector validation.
- **!10226 — Deep, merged master:** RTP Decode-As path now creates conversation state when signaling did not. The important architectural point is that an alternate entry path such as Decode As must establish the same stateful preconditions needed for coherent multi-packet dissection rather than merely making each packet individually recognizable. Existing notebook guidance already covers Decode-As entry-point preconditions and conversation/state identity, so this is corroboration rather than a new rule.
- **!10224 / !10223 / !10222:** F1AP v17.4.0 upgrade lineage. !10224 is the merged result; the closed predecessors were down-weighted rather than counted as accepted alternatives.
- **!10221, !10220, !10219, !10218, !10217, !10215, !10210, !10205, !10204, !10202:** merged generated/standards dissector upgrades (E1AP/XnAP/NGAP/X2AP/S1AP/NRPPa/NR-RRC/LPP/LTE-RRC/NAS-5GS). Scanned for review/diff lessons; no distinct durable convention beyond existing generated-source workflow guidance.
- **!10213 — Scanned/deep diff, merged master:** TECMP 10BASE-T1S and control-message expansion. Successful pipeline and straightforward protocol-field extension; no substantive human review beyond approval and no new durable rule.
- **!10209 — Scanned:** IO Graph import/export feature; primarily UI/settings functionality, no new cross-project convention extracted.
- **!10208 / !10203:** closed documentation/UI-text work; down-weighted as unmerged evidence.
- **!10207 / !10206:** GQUIC null-pointer fix/backport lineage; reinforces defensive validation but adds no new rule.
- **!10200 — Scanned:** GSM MAP subscriberId decoding correction; protocol-specific fix, no additional reusable convention.

The remaining small fixes/backports/tool cleanups in the batch were scanned for state, purpose, diff shape, and substantive discussion. They did not add a durable rule beyond conventions already present in the notebook.

## Notebook maintenance

No convention file was changed in this run. The strongest evidence independently corroborates conventions already present for Decode-As/stateful entry paths, capture-backed dissector validation, pre-submit dissector checks, and keeping unrelated changes out of an MR. Avoiding duplicate guidance was preferred over adding redundant bullets.

## Continuation

At corpus commit `08420a2f7851841b7a38714f642987c8cd167184`, the next descending candidates in the populated block begin at !10199. Future runs must rebuild the reviewed set from all ledgers and `reviewed-mrs.md` rather than assuming numeric continuity, especially because the corpus currently has a large unpopulated gap between !10299 and !15935.
