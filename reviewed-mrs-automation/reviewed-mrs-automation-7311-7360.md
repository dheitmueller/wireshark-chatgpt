# Reviewed MR automation ledger: !7311-!7360

Corpus: `dheitmueller/wireshark-corpus-mrs@ddcaa22b51c68f594e425a23388c3a2086813054`

Notebook base: `automation/mr-review-7361-7410-complete`

Reviewed in this run: **50** merge requests, selected by exact reviewed-set subtraction while continuing newest-to-oldest. The batch contains **49 merged** MRs and **1 closed/unmerged** MR: !7351.

## Tracking consulted before selection

The authoritative predecessor branch was inspected before selecting this batch.

- The `reviewed-mrs-automation/` directory contained **469 entries**.
- No standard exact-ledger filename overlaps !7311-!7360.
- `reviewed-mrs.md` contains no !7311-!7360 review entry.
- All **18** special/irregular reviewed-MR tracking files (aggregate, gap/backfill/noncontiguous/exact-list/special ledgers, including the supplemental aggregate ledger) were checked by exact candidate number; none contains !7311-!7360.
- `reviewed-mrs-automation/reviewed-mrs-automation-7361-7410.md` stops at !7361 and records !7360 only as the next unreviewed frontier probe.
- `reviewed-mrs-automation/reviewed-mrs-automation-17571-17620.md` was re-opened and still contains exactly all 50 members of the historical !17571-!17620 batch; that batch remains preserved and counted.

The corpus commit is unchanged from the preceding run. Every MR !7311 through !7360 exists in the corpus, so after exact tracking checks the fifty highest-numbered previously unreviewed MRs are exactly this interval.

## Exact reviewed MR set

!7360, !7359, !7358, !7357, !7356, !7355, !7354, !7353, !7352, !7351  
!7350, !7349, !7348, !7347, !7346, !7345, !7344, !7343, !7342, !7341  
!7340, !7339, !7338, !7337, !7336, !7335, !7334, !7333, !7332, !7331  
!7330, !7329, !7328, !7327, !7326, !7325, !7324, !7323, !7322, !7321  
!7320, !7319, !7318, !7317, !7316, !7315, !7314, !7313, !7312, !7311

Count: **50 unique MRs**.

## Weighting

Merged work is treated as primary implementation evidence. Closed !7351 is retained only as historical/scanned evidence and is not treated as an accepted design precedent. Maintainer-authored/reviewed work by Guy Harris, John Thacker, Tomasz Moń, Roland Knall, Alexis La Goutte, Anders Broman, Gerald Combs, and others is weighted according to the authority and specificity of the discussion.

!7348 is a special negative/superseded case: it merged, but Guy Harris later explicitly identified its new `proto_tree_add_bitmask_list_ret_uint64()` contract as broken when `tree == NULL`, tied it to bug #18203, and pointed to merged !7432 as the fix. Its implementation is therefore not treated as positive precedent; the review history is high-value evidence for the tree-independent return-value contract.

## Next frontier

!7310 exists in the corpus (`SOME/IP: Make uats much more robust against faulty configs (BUGFIX)`) and is merged on master. It was inspected only as the next descending frontier and is **not** counted as reviewed in this run.
