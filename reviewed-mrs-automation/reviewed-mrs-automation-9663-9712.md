# Automated MR review ledger: !9663–!9712

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Review direction: newest available previously unreviewed MRs toward older MRs.

Before selecting this batch, review tracking on the latest review branch was consulted, including `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, the previous per-run ledger `reviewed-mrs-automation/reviewed-mrs-automation-9713-9762.md`, and the historical `reviewed-mrs-automation/reviewed-mrs-automation-17571-17620.md` ledger. Candidate MR numbers were checked by explicit membership; none of !9663–!9712 appeared as already reviewed. The historical !17571–!17620 ledger was re-opened and confirmed to contain exactly 50 unique MRs.

Exactly 50 previously unreviewed MRs were reviewed in this run. Status mix: **48 merged, 2 closed/unmerged (!9712 and !9708)**. Closed/unmerged work was down-weighted.

## Exact reviewed MR set

!9712, !9711, !9710, !9709, !9708, !9707, !9706, !9705, !9704, !9703, !9702, !9701, !9700, !9699, !9698, !9697, !9696, !9695, !9694, !9693, !9692, !9691, !9690, !9689, !9688, !9687, !9686, !9685, !9684, !9683, !9682, !9681, !9680, !9679, !9678, !9677, !9676, !9675, !9674, !9673, !9672, !9671, !9670, !9669, !9668, !9667, !9666, !9665, !9664, !9663

Count: **50 unique MRs**. Minimum: **!9663**. Maximum: **!9712**.

## Outcome notes

- !9712 was a closed CI experiment by Gerald Combs; prefetching Homebrew packages did not materially improve prerequisite installation time, so it was not retained as an accepted implementation convention.
- !9708 was a closed MSYS2 runtime-detection experiment; João Valverde noted that the approach was probably unnecessary when using CPack. Merged !9709 is the accepted compile-time/platform-detection direction.
- All remaining MRs in the exact set were merged.
