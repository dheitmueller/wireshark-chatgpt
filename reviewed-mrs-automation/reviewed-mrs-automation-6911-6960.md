# Wireshark MR review automation: !6911–!6960

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Notebook base commit: `2f09838d7b840aa4554241b95f2be998183ff449`

Selection rule: the fifty highest-numbered merge requests present in the corpus that were not already recorded as reviewed, working backward from newest to oldest.

## Review-tracking reconciliation

- 405 reviewed-MR ledger/tracking files were present across `reviewed-mrs.md` and `reviewed-mrs-automation/reviewed-mrs-automation*.md`.
- No standard exact-range ledger overlapped !6911–!6960.
- The irregular gap/backfill/noncontiguous/exact-list ledgers were enumerated; none covers this low-numbered batch.
- Both aggregate trackers, `reviewed-mrs.md` and `reviewed-mrs-automation/reviewed-mrs-automation.md`, were checked for every candidate !6911–!6960; neither contained a candidate.
- The preceding !6961–!7010 ledger mentions !6960 only as the next-frontier probe and explicitly does not count it as reviewed.
- The historical !17571–!17620 exact ledger was re-opened and independently validated as 50 unique reviewed merge requests with no omissions. It remains preserved and counted.

## Exact reviewed set

!6960, !6959, !6958, !6957, !6956, !6955, !6954, !6953, !6952, !6951,
!6950, !6949, !6948, !6947, !6946, !6945, !6944, !6943, !6942, !6941,
!6940, !6939, !6938, !6937, !6936, !6935, !6934, !6933, !6932, !6931,
!6930, !6929, !6928, !6927, !6926, !6925, !6924, !6923, !6922, !6921,
!6920, !6919, !6918, !6917, !6916, !6915, !6914, !6913, !6912, !6911.

## Validation

- Exact reviewed entries: **50**
- Unique MR numbers: **50**
- Highest: **!6960**
- Lowest: **!6911**
- Merged: **47**
- Closed/unmerged: **2** — !6960, !6953
- Open/draft: **1** — !6918
- Corpus commit used for every review: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Previously reviewed !17571–!17620 batch remains preserved and counted as 50 MRs.

## Next frontier

!6910 exists in the same corpus snapshot (`DVB-S2: Only add the rolloff value once`, merged). It was inspected only to establish that the corpus continues below this run and is **not** counted as reviewed here.
