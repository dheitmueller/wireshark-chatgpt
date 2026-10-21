# Wireshark MR review ledger — !1–!7

- Model: GPT-5.6 Sol
- Corpus: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook predecessor: `automation/mr-review-8-57-authoritative-final`
- Selection direction: descending MR number
- Reviewed count: **7**
- Merged: **6** (!7, !5, !4, !3, !2, !1)
- Closed / unmerged: **1** (!6)
- Corpus lower frontier: `mr_0.json` is absent; !1 is the lowest available MR record.

Before selection, the available notebook tracking was reconciled. The full `reviewed-mrs-automation/` inventory on the predecessor branch was enumerated (457 tracking/support files), the main `reviewed-mrs.md` and supplemental `reviewed-mrs-automation/reviewed-mrs-automation.md` ledgers were checked, the immediately preceding exact `ledger-8-57.md` was checked, and the dedicated !17571–!17620 historical ledger was revalidated. Exact searches in those aggregate/current ledgers found no prior completed review of !1–!7.

The historical file `reviewed-mrs-automation/reviewed-mrs-automation-17571-17620.md` still contains all **50 unique** MRs !17571 through !17620.

## Exact reviewed MR list

- !7
- !6
- !5
- !4
- !3
- !2
- !1

## Weighting

Merged master changes are treated as the strongest implementation evidence. Closed !6 is not an implementation exemplar; its Dario Lombardo review is retained as submission-workflow evidence because it directly explains why the first submission was replaced by merged !7.

## Corpus frontier

The corpus contains usable MR records through !1. `mr_0.json` is not present. No lower MR is available in this corpus snapshot.

## VANC check

No SMPTE ST 291 / VANC packet type was encountered in !1–!7.
