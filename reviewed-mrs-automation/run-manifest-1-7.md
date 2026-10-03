# Wireshark MR review run manifest — !1–!7

## Inputs

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook repository: `dheitmueller/wireshark-chatgpt`
- Notebook predecessor: `automation/mr-review-8-57-authoritative-final`
- Model requirement satisfied: GPT-5.6 Sol

## Review-tracking reconciliation

The predecessor branch's `reviewed-mrs-automation/` directory was enumerated before selection. It contains 457 tracking, findings, convention, and manifest files. Candidate selection was not inferred from ranges alone. The aggregate `reviewed-mrs.md`, supplemental `reviewed-mrs-automation/reviewed-mrs-automation.md`, predecessor `ledger-8-57.md`, and historical !17571–!17620 ledger were consulted explicitly; !1–!7 had no completed-review hit in those current aggregate/exact trackers.

The !17571–!17620 ledger was revalidated as 50 unique MR numbers, preserving that batch.

## Selection

The next descending frontier after !8 was !7. Corpus records exist for !7, !6, !5, !4, !3, !2, and !1. `mr_0.json` returns not found. Therefore only seven previously unreviewed MRs remain available, fewer than the requested maximum of fifty.

Exact reviewed set: **!7, !6, !5, !4, !3, !2, !1**.

## Outcome

- Merged: 6
- Closed/unmerged: 1 (!6)
- Open: 0
- Lower corpus records remaining: 0

## Notebook updates

Durable findings are recorded in:
- `reviewed-mrs-automation/review-findings-1-7.md`
- `reviewed-mrs-automation/conventions-1-7.md`
- `conditional-compilation-conventions.md`
- `testing-fuzzing.md`

No SMPTE ST 291 / VANC packet type was encountered.
