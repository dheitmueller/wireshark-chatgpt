# Automated MR review ledger: !9713–!9762

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Review direction: newest available previously unreviewed MRs toward older MRs.

Before selecting this batch, the accumulated review state on the latest review branch was consulted, including `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, the previous per-run ledger `reviewed-mrs-automation-9763-9812.md`, and the historical `reviewed-mrs-automation-17571-17620.md` ledger. Candidate MR numbers were checked by explicit membership rather than inferred interval coverage. None of !9713–!9762 appeared as already reviewed. The historical !17571–!17620 ledger was re-opened and verified to contain exactly 50 unique MRs, minimum !17571 and maximum !17620.

Exactly 50 previously unreviewed MRs were reviewed in this run. Status mix: **47 merged, 1 open (!9742), 2 closed/unmerged (!9721 and !9718)**. Merged MRs were weighted more heavily than open, abandoned, or superseded work. Highly authoritative maintainer evidence was weighted accordingly, including Guy Harris, John Thacker, Gilbert Ramirez, Stig Bjørlykke, Martin Mathieson, João Valverde, Gerald Combs, and others.

## Exact reviewed MR set

!9762, !9761, !9760, !9759, !9758, !9757, !9756, !9755, !9754, !9753, !9752, !9751, !9750, !9749, !9748, !9747, !9746, !9745, !9744, !9743, !9742, !9741, !9740, !9739, !9738, !9737, !9736, !9735, !9734, !9733, !9732, !9731, !9730, !9729, !9728, !9727, !9726, !9725, !9724, !9723, !9722, !9721, !9720, !9719, !9718, !9717, !9716, !9715, !9714, !9713

Count: **50 unique MRs**. Minimum: **!9713**. Maximum: **!9762**.

## Outcome notes

- !9742 remained an open draft in the corpus snapshot and was used only as lower-weight evidence. Its Q-Block work was deliberately split from the merged bug fix !9731.
- !9721 was closed/unmerged with no retained diff; merged !9722 is the accepted RTPS compressed-data fix and therefore carries the implementation weight.
- !9718 was closed after review established that the proposed extra libgcrypt linkage was a workaround for a local dependency mismatch rather than the correct project fix.
- All other MRs in the exact set were merged.
