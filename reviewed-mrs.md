# Reviewed Wireshark Merge Requests

This is the persistent ledger of Wireshark merge requests already examined for this engineering notebook. Consult this file before selecting MRs for future review so compute is not wasted repeating work.

An MR may be revisited when there is a reason: new substantive review comments, significant new commits, a changed merge outcome, a need to verify an earlier conclusion, or an explicit user request. Record revisits rather than silently treating them as new reviews.

## Review status meanings

- **Deep** — diff/relevant implementation and substantive review discussion were examined sufficiently to extract engineering lessons.
- **Discussion-focused** — useful human review discussion was examined, but the complete code/diff was not necessarily available.
- **Partial** — some useful material was examined, but access or scope prevented a complete review. A later revisit may be worthwhile.
- **Probe only** — attempted access but did not obtain enough material to count as a substantive review. These should not normally block a future review.

## Ledger

| MR | Status | Notes |
|---|---|---|
| !26390 | Deep (initial revision) | ST 2110-40 plus related VANC dissectors. Reviewed exported JSON with all 12 changed files, 3 commits, diff-version metadata and six human comments from Anders Broman. Lessons: protocol-prefix `hf_`/`ett_` identifiers; prefer tvbuff/proto bit APIs over custom bit extraction; sample pcaps expected; squash focused development history; small related protocols can share a source file. MR remains open and review threads unresolved in this snapshot, so revisit after new commits/comments. |
| !26386 | Probe only | Visible in recent MR listing but individual MR content was not accessible during the 2026-09-07 calibration run. |
| !26378 | Probe only | Visible in recent MR listing but individual MR content was not accessible during the 2026-09-07 calibration run. |
| !26376 | Probe only | Visible in recent MR listing but individual MR content was not accessible during the 2026-09-07 calibration run. |
| !26374 | Probe only | Visible in recent MR listing but individual MR content was not accessible during the 2026-09-07 calibration run. |
| !26361 | Partial | TCP unusual-flags change; MR page/relevant test context accessible during calibration run. No major durable reviewer convention extracted yet. |
| !26358 | Probe only | Visible in recent MR listing but individual MR content was not accessible during the 2026-09-07 calibration run. |
| !26323 | Partial | HyperDHT dissector; malformed/truncation and heuristic-recognition testing observed. No major durable reviewer convention extracted yet. |
| !26313 | Probe only | Visible in recent MR listing but individual MR content was not accessible during the 2026-09-07 calibration run. |
| !26280 | Probe only | Visible in recent MR listing but individual MR content was not accessible during the 2026-09-07 calibration run. |
| !26245 | Probe only | Visible in recent MR listing but individual MR content was not accessible during the 2026-09-07 calibration run. |
| !22662 | Discussion-focused | NTP over PTP. Michael Mann requested a sample capture. Review/discussion exposed first-pass dissection problems caused by `if (tree)` guards and demonstrated splitting a prerequisite fix into separate MR !22663. Lessons recorded in `review-patterns.md`. |
| !22208 | Discussion-focused | Alexis La Goutte requested use of a named topic branch rather than submitting from fork `master`; contributor-workflow lesson recorded in `review-patterns.md`. |
| !20793 | Discussion-focused | John Thacker recognized an incremental prerequisite change even though reassembled content was not yet dissected; lesson recorded in `review-patterns.md`. |
| !16672 | Discussion-focused | Older MR with substantive Guy Harris review threads identified as valuable evidence. Revisit for deeper extraction if not already fully captured in `review-patterns.md`. |
| !19481 | Partial | MR activity/reviewer discussion was confirmed accessible during initial GitLab-access testing. No durable lesson yet recorded; may be worth revisiting. |

## Selection guidance

For future mining, prioritize information density over recency. Prefer accessible MRs with substantive human review, especially reviews by Guy Harris and other core maintainers; dissector/libwireshark API changes; malformed/truncation handling; field/filter semantics; registration/handoff; reassembly; RTP/media; tests; and fuzzing.

For MRs marked `Deep (initial revision)` or similar, do not re-review an unchanged snapshot. Revisit only when its `updated_at`, head SHA, diff-version set, discussion set, or merge state has changed.

Do not spend repeated retrieval attempts on very recent MRs that are visible in listings but not yet individually indexed. Record them as `Probe only` and move on; revisit later.
