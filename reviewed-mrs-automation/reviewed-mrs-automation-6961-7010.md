# Wireshark MR review automation: !6961–!7010

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Notebook base commit: `0da4275b22c3fc022ddecb60ee47e33d307950ea`

Selection rule: the fifty highest-numbered merge requests present in the corpus that were not already recorded as reviewed, working backward from newest to oldest.

## Review-tracking reconciliation

- 405 actual ledger/tracking files were present across `reviewed-mrs.md` and `reviewed-mrs-automation/reviewed-mrs-automation*.md`.
- 387 were standard exact range ledgers. The lowest standard exact range already reviewed was `!7011–!7060`.
- The irregular gap/backfill/noncontiguous/exact-list ledgers were enumerated; none covered any MR in `!6961–!7010`.
- Both aggregate trackers were checked explicitly for every candidate number `!6961–!7010`; neither contained a candidate.
- The preceding `!7011–!7060` ledger mentioned `!7010` only as a frontier probe and explicitly did not count it as reviewed.
- The historical `!17571–!17620` exact ledger was re-opened and preserved as 50 previously reviewed unique merge requests.

## Exact reviewed set

!7010, !7009, !7008, !7007, !7006, !7005, !7004, !7003, !7002, !7001,
!7000, !6999, !6998, !6997, !6996, !6995, !6994, !6993, !6992, !6991,
!6990, !6989, !6988, !6987, !6986, !6985, !6984, !6983, !6982, !6981,
!6980, !6979, !6978, !6977, !6976, !6975, !6974, !6973, !6972, !6971,
!6970, !6969, !6968, !6967, !6966, !6965, !6964, !6963, !6962, !6961.

## Validation

- Exact reviewed entries: **50**
- Unique MR numbers: **50**
- Highest: **!7010**
- Lowest: **!6961**
- Merged: **46**
- Closed/unmerged: **4** — !6997, !6975, !6967, !6963
- Corpus commit used for every review: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Previously reviewed !17571–!17620 batch remains preserved and counted as 50 MRs.

## Next frontier

`!6960` exists in the same corpus snapshot (`zbee: Add 9 zcl frames`, closed). It was inspected only to establish that the corpus continues below this run and is **not** counted as reviewed here.
