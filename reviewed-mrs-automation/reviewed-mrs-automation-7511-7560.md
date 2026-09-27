# Reviewed MR automation ledger: !7511-!7560

Corpus: dheitmueller/wireshark-corpus-mrs@ddcaa22b51c68f594e425a23388c3a2086813054

Notebook base: 127e53fc4f9a1937e0dc70122d698966d4b9b883

Reviewed: **50** (**47 merged**, **3 closed/unmerged**: !7543, !7537, !7516).

Before selection, persistent tracking was reconciled from reviewed-mrs.md, the supplemental reviewed-mrs-automation/reviewed-mrs-automation.md tracker, the immediately preceding exact !7561-!7610 ledger, and the historical !17571-!17620 ledger. The historical ledger still contains exactly 50 unique MR numbers and remains preserved and counted.

Each candidate !7560 through !7511 was checked individually against the searchable reviewed-MR tracking; none had an existing reviewed-MR tracking hit. The prior mention of !7560 was only a frontier probe. Selection therefore used explicit MR membership rather than assuming any numeric interval was wholly reviewed.

## Exact reviewed MR set

!7560, !7559, !7558, !7557, !7556, !7555, !7554, !7553, !7552, !7551,
!7550, !7549, !7548, !7547, !7546, !7545, !7544, !7543, !7542, !7541,
!7540, !7539, !7538, !7537, !7536, !7535, !7534, !7533, !7532, !7531,
!7530, !7529, !7528, !7527, !7526, !7525, !7524, !7523, !7522, !7521,
!7520, !7519, !7518, !7517, !7516, !7515, !7514, !7513, !7512, !7511.

Count: **50 unique MRs**.

## Weighting

Merged master MRs were treated as the strongest accepted evidence. Stable-branch backports were mainly corroborative. Maintainer-authored work and substantive maintainer review were weighted strongly, especially John Thacker and other established maintainers/reviewers.

Closed !7543 was down-weighted because the proposed implementation was superseded, although John Thacker's discussion is useful architecture evidence. Closed !7537 and !7516 were exploratory changes explicitly abandoned by their authors.

## Corpus anomaly

For !7511, the corpus title/description says "Docbook: wslua_util → wslua_utility.", but the recorded commit and sole diff are "appveyor: We no longer require Perl." against appveyor.yml. This run reviewed the actual recorded commit/diff while preserving the corpus MR identity.
