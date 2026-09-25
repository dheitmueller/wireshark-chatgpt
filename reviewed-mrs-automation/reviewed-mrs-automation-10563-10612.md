# Automated MR review ledger: !10563–!10612

Reviewed using GPT-5.6 Sol.

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Exactly 50 previously unreviewed merge requests were reviewed. Selection was rebuilt from the notebook review tracking, including `reviewed-mrs.md`, the per-run inventory under `reviewed-mrs-automation/`, recent exact ledgers above !10612, the discontiguous !25827 backfill ledger, the special !26566 ledger, and the historical !17571–!17620 ledger. The historical batch was explicitly re-opened and still contains exactly 50 reviewed MRs. Candidate membership for !10612 through !10563 was checked individually; none was already tracked as reviewed.

Status mix: **49 merged, 1 closed/unmerged (!10604)**. Merged master MRs were weighted most heavily; backports mainly corroborated their master changes; !10604 was down-weighted.

## Exact reviewed MR set

!10612 !10611 !10610 !10609 !10608 !10607 !10606 !10605 !10604 !10603
!10602 !10601 !10600 !10599 !10598 !10597 !10596 !10595 !10594 !10593
!10592 !10591 !10590 !10589 !10588 !10587 !10586 !10585 !10584 !10583
!10582 !10581 !10580 !10579 !10578 !10577 !10576 !10575 !10574 !10573
!10572 !10571 !10570 !10569 !10568 !10567 !10566 !10565 !10564 !10563

Count: **50 unique MRs**.

## Promoted findings

- !10611: first-pass-only negotiated-version learning from both TDS peers.
- !10605: typed-item checker fixes must audit every use of a shared `hf_` field before changing its metadata.
- !10588, corroborated by !10591/!10592: preserve the coordinate system of parser return values; an absolute next offset is not a remaining length.
- !10584: expose semantically distinct protocol components as real filterable/exportable fields rather than only formatted composite text.
- !10565: use protocol-relative occurrence identity rather than total protocol-stack layer number for nested same-protocol state that must survive redissection.

Full review notes are in `review-findings-10563-10612.md`.

## Next frontier

The corpus is not exhausted. !10562 exists at the same corpus commit and is merged. It was inspected only as the next-frontier probe and was not reviewed or counted in this run.
