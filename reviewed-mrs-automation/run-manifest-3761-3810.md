# Run manifest: Wireshark MR review !3761-!3810

- Date: 2026-09-30
- Model: GPT-5.6 Sol
- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook predecessor branch: `automation/mr-review-3811-3860-authoritative-final`
- Notebook predecessor commit: `867ac4fcf69af19c0338ad656e9018a31cb14773`
- Selected batch: exactly !3810 through !3761 (50 unique MRs)
- Outcomes: 46 merged, 4 closed/unmerged (!3804, !3803, !3768, !3762)
- Historical preservation check: `reviewed-mrs-automation-17571-17620.md` contains 50 unique MR rows and remains counted.
- Tracking reconciliation: inventoried all files under `reviewed-mrs-automation/`; checked root `reviewed-mrs.md`, the aggregate automation ledger, and the immediately preceding exact ledger for exact candidate membership. No selected candidate was already recorded as reviewed.
- Selection direction: descending from the previous frontier; no candidate was inferred reviewed merely from a numeric range filename.
- VANC/SMPTE 291 types encountered: none.
- Frontier probe after the batch: !3760 exists in the corpus, is merged on master, is titled `HTTP3: Add Settings dissection`, and was not counted as reviewed.

Topical notebook promotions in this run:
- `allocator-scope-conventions.md`
- `platform-build-environment-conventions.md`
- `typed-item-checker-conventions.md`
- `library-layering-conventions.md`
- `generated-code-conventions.md`

Per-run authoritative artifacts:
- `reviewed-mrs-automation/ledger-3761-3810.md`
- `reviewed-mrs-automation/review-findings-3761-3810.md`
- `reviewed-mrs-automation/conventions-3761-3810.md`
- `reviewed-mrs-automation/run-manifest-3761-3810.md`
