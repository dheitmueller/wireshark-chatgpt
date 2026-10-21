# Wireshark MR review ledger — !8–!57

- Model: GPT-5.6 Sol
- Corpus: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook predecessor: `automation/mr-review-58-107-authoritative`
- Selection direction: descending MR number
- Reviewed count: **50**
- Merged: **31**
- Closed / unmerged: **18** (!54, !46, !42, !41, !40, !37, !36, !35, !34, !32, !29, !23, !20, !19, !16, !15, !10, !9)
- Open snapshot: **1** (!13)

Before selection, the available notebook tracking was reconciled, including `reviewed-mrs.md`, the supplemental automation tracking, the per-run ledgers on the predecessor branch, and the historical !17571–!17620 tracking file. Candidate membership was treated by individual MR number rather than inferred range coverage. The corpus HEAD was rechecked and remained `ddcaa22b51c68f594e425a23388c3a2086813054`, so no newly-added higher-numbered corpus holes displaced the established frontier.

The historical file `reviewed-mrs-automation/reviewed-mrs-automation-17571-17620.md` was re-opened. It contains all **50 unique** MRs !17571 through !17620; repeated references in prose/table content do not change the unique reviewed set.

## Exact reviewed MR list

- !57
- !56
- !55
- !54
- !53
- !52
- !51
- !50
- !49
- !48
- !47
- !46
- !45
- !44
- !43
- !42
- !41
- !40
- !39
- !38
- !37
- !36
- !35
- !34
- !33
- !32
- !31
- !30
- !29
- !28
- !27
- !26
- !25
- !24
- !23
- !22
- !21
- !20
- !19
- !18
- !17
- !16
- !15
- !14
- !13
- !12
- !11
- !10
- !9
- !8

## Weighting

Merged master changes are the strongest implementation evidence. Stable-branch backports are corroborative unless their branch-specific adaptation adds a distinct lesson. Closed, abandoned, or superseded changes are not implementation exemplars, although substantive maintainer review remains useful evidence. !13 was still open in the corpus snapshot and is therefore provisional.

## VANC check

No SMPTE ST 291 / VANC packet type was encountered in !8–!57. Repository searches for `VANC` and `SMPTE 291` returned no MR in this batch. A broad `ST 291` text search produced number/string false positives rather than VANC content.
