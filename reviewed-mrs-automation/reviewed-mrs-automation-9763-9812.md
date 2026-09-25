# Automated MR review ledger: !9763–!9812

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Review direction: newest available previously unreviewed MRs toward older MRs.

Before selecting this batch, the available review tracking in `dheitmueller/wireshark-chatgpt` was consulted, including `reviewed-mrs.md`, the aggregate `reviewed-mrs-automation/reviewed-mrs-automation.md`, the cumulative per-run state carried by the latest review branch, and the historical `reviewed-mrs-automation/reviewed-mrs-automation-17571-17620.md` ledger. The historical !17571–!17620 ledger was re-opened and verified to contain exactly 50 unique MR numbers with no omissions. Candidate selection was by explicit MR-number membership rather than by assuming a numeric interval was wholly reviewed. The prior mention of !9812 was only a frontier probe and did not count as a review.

Exactly 50 previously unreviewed MRs were reviewed in this run. Status mix: **48 merged, 1 open (!9799), 1 closed/unmerged (!9778)**. Merged MRs were weighted more heavily than open, abandoned, or superseded work. Maintainer review was weighted by authority, with direct guidance from people such as Guy Harris, John Thacker, Gilbert Ramirez, João Valverde, and Martin Mathieson receiving appropriate weight.

## Exact reviewed MR set

!9812, !9811, !9810, !9809, !9808, !9807, !9806, !9805, !9804, !9803, !9802, !9801, !9800, !9799, !9798, !9797, !9796, !9795, !9794, !9793, !9792, !9791, !9790, !9789, !9788, !9787, !9786, !9785, !9784, !9783, !9782, !9781, !9780, !9779, !9778, !9777, !9776, !9775, !9774, !9773, !9772, !9771, !9770, !9769, !9768, !9767, !9766, !9765, !9764, !9763

Count: **50 unique MRs**. Minimum: **!9763**. Maximum: **!9812**.

## Outcome notes

- !9799 remained open in the corpus snapshot and was used only as lower-weight review evidence.
- !9778 was closed/unmerged and appears superseded by !9824; it was counted in the exact reviewed set but not treated as an accepted implementation exemplar.
- All other MRs in the exact set were merged.
