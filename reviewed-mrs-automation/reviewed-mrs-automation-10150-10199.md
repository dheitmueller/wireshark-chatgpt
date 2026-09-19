# Wireshark MR review automation: !10150-!10199

Corpus commit: `08420a2f7851841b7a38714f642987c8cd167184`

Review direction: descending from newest available unreviewed MR toward older MRs.

This run rebuilt the already-reviewed set from the available `reviewed-mrs-automation/` ledgers plus `reviewed-mrs.md`, preserving the previously reviewed !17571-!17620 batch. The fifty highest-numbered MRs present in the corpus and not already recorded as reviewed were !10199 through !10150. No assumption was made that gaps in the corpus or gaps between ledgers were reviewed.

## Exact reviewed set

!10199, !10198, !10197, !10196, !10195, !10194, !10193, !10192, !10191, !10190,
!10189, !10188, !10187, !10186, !10185, !10184, !10183, !10182, !10181, !10180,
!10179, !10178, !10177, !10176, !10175, !10174, !10173, !10172, !10171, !10170,
!10169, !10168, !10167, !10166, !10165, !10164, !10163, !10162, !10161, !10160,
!10159, !10158, !10157, !10156, !10155, !10154, !10153, !10152, !10151, !10150.

Count: **50**.

## Durable findings

- **!10154 — XOT: Fix use of `tcp_dissect_pdus()` — Deep.** Merged, authored and merged by John Thacker. The old XOT path could ask TCP for another segment and then still reject the packet heuristically. The accepted fix performs recognition before stream ownership, binds the conversation after recognition, and uses a non-heuristic TCP dissector with `tcp_dissect_pdus()` thereafter. This is strong API-contract evidence and is recorded in `tcp-desegmentation-conventions.md`.
- **!10150 — Restrict allowed characters in dissector names — Deep.** Merged, authored and merged by John Thacker. Registered dissector names are machine-facing identifiers and must use the restricted identifier character set; human-friendly text belongs in the separate description. The change fixes real lookup/UAT parsing problems caused by spaces and other characters. This reinforces treating registration names as stable identifiers rather than labels.
- **!10166 — Sharkd: return JSON error for bad download tokens — Deep.** Merged. Sharkd must return a JSON-RPC response for client input even on validation failures. The patch adds explicit errors for missing/unrecognized tokens and regression tests for both cases. Strong corroboration for testing negative/error paths at the externally visible protocol boundary.
- **!10169 — MaxMindDB: Reap mmdbresolve process — Deep.** Merged, authored and merged by John Thacker. `G_SPAWN_DO_NOT_REAP_CHILD` transfers lifecycle responsibility to the caller; configuration-driven stop/restart paths must reap the child, not just rely on process shutdown. The accepted code handles `EINTR`, tolerates `ECHILD`, logs unexpected failures, and avoids zombie accumulation.
- **!10158 — Draft BFCP validation — Scanned, closed/unmerged.** The branch ultimately contained a revert and an empty diff. It is deliberately down-weighted; !10159 is the later substantive submission and is stronger evidence.
- **!10174 / !10175 — CI fuzz-time tuning — Scanned.** Merged CI changes adjusting ASan/Valgrind fuzz scheduling. Useful historical evidence that fuzz duration/resource allocation is deliberately tuned, but no new coding convention extracted.
- **!10152 — ORAN FH CUS array-index check — Scanned.** Merged defensive bounds fix; corroborates existing packet-controlled-index/capacity guidance.
- **!10163 — extcap/language preference dangling-GSL warning fix — Scanned.** Merged compiler-hygiene/temporary-lifetime cleanup; corroborates the existing warning-as-error convention.
- **!10198 — GQUIC null-pointer exception fix — Scanned.** Merged defensive fix for a nullable information path; no distinct new rule beyond existing entry/precondition guidance.
- **!10197, !10196, !10195 — automatic update MRs — Scanned.** Merged generated/data/translation maintenance; no durable coding convention.

The remaining MRs in the exact set were examined for metadata, outcome, purpose, available discussion, and relevant diff context. Merged implementation evidence was weighted above closed/draft/superseded material; routine codepoint/list/backport/formatting changes were not promoted into notebook rules without a durable lesson.

## Continuation

Subject to rebuilding the complete already-reviewed set on the next run, the next descending candidate in the currently populated corpus is **!10149**. Do not infer that absent higher-numbered corpus ranges have been reviewed merely because this run continued at !10199.
