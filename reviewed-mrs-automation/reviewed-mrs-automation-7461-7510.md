# Reviewed MR automation ledger: !7461-!7510

Corpus: dheitmueller/wireshark-corpus-mrs@ddcaa22b51c68f594e425a23388c3a2086813054

Notebook base: 71351e54f4e3419c35966ced2043fa974c1bab36

Reviewed: **50** (**47 merged**, **3 closed/unmerged**: !7500, !7469, !7467).

Before selection, tracking was reconciled from `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, the full `reviewed-mrs-automation/` directory inventory (including gap/backfill/noncontiguous ledgers), the immediately preceding exact !7511-!7560 ledger, and the historical !17571-!17620 ledger. Neither the aggregate tracker nor `reviewed-mrs.md` contains any of !7461-!7510; the per-run directory has no ledger whose reviewed set overlaps this interval. The preceding exact ledger ends at !7511 and the prior mention of !7510 was a frontier probe only.

The historical !17571-!17620 ledger was re-read and independently validated as exactly **50 unique MR numbers with no missing member**, so that previously reviewed batch remains preserved and counted.

## Exact reviewed MR set

!7510, !7509, !7508, !7507, !7506, !7505, !7504, !7503, !7502, !7501.
!7500, !7499, !7498, !7497, !7496, !7495, !7494, !7493, !7492, !7491.
!7490, !7489, !7488, !7487, !7486, !7485, !7484, !7483, !7482, !7481.
!7480, !7479, !7478, !7477, !7476, !7475, !7474, !7473, !7472, !7471.
!7470, !7469, !7468, !7467, !7466, !7465, !7464, !7463, !7462, !7461.

Count: **50 unique MRs**.

## Weighting

Merged master MRs are treated as the strongest accepted implementation evidence. Stable-branch backports are corroborative unless they add distinct review information. Maintainer-authored changes and substantive maintainer review are weighted strongly, especially John Thacker, Gerald Combs, Stig Bjørlykke, Martin Mathieson, Jaap Keuter, João Valverde, Roland Knall, Alexis La Goutte, and other established maintainers/reviewers.

Closed !7500 is an experimental HTTP/3/QPACK draft explicitly superseded by !9330; its proposed implementation is not treated as accepted precedent. Closed !7469 was a short-lived Locamation build workaround that the contributor said broke the dissector, and it is outweighed by the merged !7470/!7472 sequence. Closed !7467 was superseded by the later merged !7473 submission.

## Selection frontier

The next lower MR in the corpus is !7460. It is not included in this run and is not counted as reviewed here.
