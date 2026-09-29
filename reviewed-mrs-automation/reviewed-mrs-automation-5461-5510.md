# Wireshark MR review ledger 5461-5510

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Notebook base: `2a63701a8ec5d77748c8a550f494fdf4ad8ceb3e` (`automation/mr-review-5511-5560-final`)

Reviewed exactly 50 previously unreviewed merge requests in descending order:

!5510 !5509 !5508 !5507 !5506 !5505 !5504 !5503 !5502 !5501  
!5500 !5499 !5498 !5497 !5496 !5495 !5494 !5493 !5492 !5491  
!5490 !5489 !5488 !5487 !5486 !5485 !5484 !5483 !5482 !5481  
!5480 !5479 !5478 !5477 !5476 !5475 !5474 !5473 !5472 !5471  
!5470 !5469 !5468 !5467 !5466 !5465 !5464 !5463 !5462 !5461

Outcome count: 47 merged; 2 closed/unmerged (!5506, !5475); 1 open/unmerged (!5489).

## Tracking reconciliation

- The authoritative notebook tree contains 417 ordinary exact-range ledgers. The lowest ordinary range is !5511-!5560, so no ordinary exact-range ledger overlaps this candidate batch.
- All 21 non-ordinary/aggregate/root review-tracking files were read and searched for individual membership of every candidate !5461-!5510: the irregular gap/backfill/noncontiguous/exact-list ledgers, the supplemental aggregate tracker, the reconciliation ledger, and root `reviewed-mrs.md`. None records any candidate as previously reviewed.
- The immediately preceding exact ledger, `reviewed-mrs-automation-5511-5560.md`, explicitly identifies !5510 only as a metadata-only next-frontier probe, not as a completed review.
- The historical `reviewed-mrs-automation-17571-17620.md` was re-read and still enumerates exactly 50 reviewed MRs from !17571 through !17620, so that batch remains preserved and counted.

## Evidence weighting

Merged master changes are preferred over maintained-branch backports and over open, abandoned, or superseded submissions. Direct review from established maintainers is weighted accordingly; this batch includes especially useful review from Guy Harris, John Thacker, Stig Bjørlykke, Gerald Combs, and others. Open !5489 and closed !5475/!5506 are retained only as lower-weight workflow or design-history evidence.

Next frontier: MR !5460 is reserved for a metadata-only post-run probe and is not part of this ledger.
