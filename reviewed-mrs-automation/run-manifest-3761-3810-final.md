# Final run manifest: Wireshark MR review !3761-!3810

This file supersedes `run-manifest-3761-3810.md` for run-status details.

- Date: 2026-09-30
- Model: GPT-5.6 Sol
- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook predecessor branch: `automation/mr-review-3811-3860-authoritative-final`
- Notebook predecessor commit: `867ac4fcf69af19c0338ad656e9018a31cb14773`
- Selected batch: exactly !3810 through !3761 (50 unique MRs)
- Outcomes: 46 merged, 4 closed/unmerged (!3804, !3803, !3768, !3762)
- Historical !17571-!17620 preservation: 50 entries, 50 unique, minimum !17571, maximum !17620.
- Tracking inventory: 708 files under `reviewed-mrs-automation/`, including 472 ledger/reconciliation/reviewed-MR tracking artifacts.
- Candidate-membership reconciliation: root `reviewed-mrs.md`, the aggregate automation tracker, the predecessor exact ledger, and all irregular/gap/backfill/noncontiguous/exact-list/generic trackers identified from the inventory were checked for !3761-!3810; no candidate was already recorded as reviewed.
- VANC/SMPTE 291 types encountered: none.
- Frontier probe: !3760 (`HTTP3: Add Settings dissection`) exists, is merged on master, and was not counted as reviewed.
- Corpus recheck after review: head remained `ddcaa22b51c68f594e425a23388c3a2086813054`.

Authoritative per-run artifacts:
- `reviewed-mrs-automation/ledger-3761-3810.md`
- `reviewed-mrs-automation/review-findings-3761-3810.md`
- `reviewed-mrs-automation/conventions-3761-3810.md`
- this final manifest

Topical synthesis status:
- The durable findings map naturally to `allocator-scope-conventions.md`, `platform-build-environment-conventions.md`, `typed-item-checker-conventions.md`, `library-layering-conventions.md`, and `generated-code-conventions.md`.
- An attempted direct update to an existing topical convention file was rejected by the repository mutation safety layer. The run did not bypass that protection, and existing topical files were left unchanged.
- The complete durable synthesis is therefore preserved in `conventions-3761-3810.md` and the complete evidence trail in `review-findings-3761-3810.md`.
