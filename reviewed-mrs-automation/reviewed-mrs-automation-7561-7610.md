# Reviewed MR automation ledger: !7561-!7610

Corpus: `dheitmueller/wireshark-corpus-mrs@ddcaa22b51c68f594e425a23388c3a2086813054`

Notebook base: `90a40da4a683f2db69cacbf98624781ff22ebf4a`

Reviewed: **50** (**49 merged**, **1 closed/unmerged**).

Before selection, all available review tracking on the authoritative notebook state was reconciled. This included `reviewed-mrs.md`, the supplemental aggregate tracker `reviewed-mrs-automation/reviewed-mrs-automation.md`, the complete per-run ledger directory inventory, the immediately preceding exact !7611-!7660 ledger, and the historical !17571-!17620 ledger. The historical ledger was re-opened and still contains exactly 50 unique MR numbers.

The corpus commit is unchanged from the preceding run. Candidate membership was checked for every !7610 through !7561 against the persistent ledgers rather than inferring coverage from a numeric range. None of these fifty MR numbers was already recorded as reviewed. The prior mention of !7610 in the !7611-!7660 run was only a frontier probe and did not count as review.

## Exact reviewed MR set

!7610, !7609, !7608, !7607, !7606, !7605, !7604, !7603, !7602, !7601,
!7600, !7599, !7598, !7597, !7596, !7595, !7594, !7593, !7592, !7591,
!7590, !7589, !7588, !7587, !7586, !7585, !7584, !7583, !7582, !7581,
!7580, !7579, !7578, !7577, !7576, !7575, !7574, !7573, !7572, !7571,
!7570, !7569, !7568, !7567, !7566, !7565, !7564, !7563, !7562, !7561.

Count: **50 unique MRs**.

## Weighting

Merged MRs were treated as accepted upstream evidence and weighted more heavily than the single closed draft. Maintainer-authored work and direct maintainer review were weighted strongly, especially John Thacker, Gerald Combs, Roland Knall, Alexis La Goutte, Jaap Keuter, and other established reviewers. Closed !7610 contributes useful TCP review reasoning from John Thacker but is not treated as an accepted implementation pattern.
