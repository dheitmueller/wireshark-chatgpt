# Wireshark MR review ledger: !5961–!6010

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`
Notebook base commit: `c4a5d922ce92b93301fe25fca657532f4fba07c8`

## Selection and reconciliation

This run built the already-reviewed set before selecting candidates. On the authoritative notebook base, `reviewed-mrs-automation/` contained 407 ordinary exact-range ledgers plus 16 irregular/gap/backfill/noncontiguous/exact-list trackers. `reviewed-mrs.md` was also checked. None recorded any MR from !5961 through !6010 as already reviewed. The previously reviewed !17571–!17620 batch remains preserved and counted as 50 reviewed MRs.

The selected set is therefore the fifty highest-numbered previously unreviewed MRs present in this corpus snapshot. The range happens to be contiguous, but it was selected after reconciliation rather than inferred from range boundaries.

## Exact reviewed set

Reviewed exactly these 50 MRs, newest to oldest:

!6010, !6009, !6008, !6007, !6006, !6005, !6004, !6003, !6002, !6001,
!6000, !5999, !5998, !5997, !5996, !5995, !5994, !5993, !5992, !5991,
!5990, !5989, !5988, !5987, !5986, !5985, !5984, !5983, !5982, !5981,
!5980, !5979, !5978, !5977, !5976, !5975, !5974, !5973, !5972, !5971,
!5970, !5969, !5968, !5967, !5966, !5965, !5964, !5963, !5962, !5961.

Outcome: 47 merged; 3 closed/unmerged (!5986, !5970, !5968). Closed work was down-weighted and used only as duplicate, rejected-design, or review-history evidence.

## Continuity

!5960 ("GTP: Add some undecoded IEs") exists in the corpus and is merged. It was inspected only as the next-frontier metadata probe and is **not** counted as reviewed in this run.

The corpus is therefore not exhausted at `ddcaa22b51c68f594e425a23388c3a2086813054`.
