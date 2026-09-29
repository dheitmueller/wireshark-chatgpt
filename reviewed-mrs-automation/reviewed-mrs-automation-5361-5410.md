# Wireshark MR review ledger 5361-5410

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Notebook base: `a4ff1710581ac3e008746bf8494fb497e9ba084a` (`automation/mr-review-5411-5460-authoritative`)

Reviewed exactly 50 previously unreviewed merge requests in descending order:

!5410 !5409 !5408 !5407 !5406 !5405 !5404 !5403 !5402 !5401
!5400 !5399 !5398 !5397 !5396 !5395 !5394 !5393 !5392 !5391
!5390 !5389 !5388 !5387 !5386 !5385 !5384 !5383 !5382 !5381
!5380 !5379 !5378 !5377 !5376 !5375 !5374 !5373 !5372 !5371
!5370 !5369 !5368 !5367 !5366 !5365 !5364 !5363 !5362 !5361

Outcome count: 47 merged; 3 closed/unmerged (!5380, !5366, !5361).

## Tracking reconciliation

- The authoritative predecessor ledger, `reviewed-mrs-automation-5411-5460.md`, records !5410 only as a metadata-only frontier probe, not as reviewed.
- That predecessor state already reconciled 418 ordinary exact-range ledgers plus all 22 non-ordinary/aggregate/root tracking files and established !5411 as the lowest reviewed ordinary frontier. The exact per-MR membership set was preserved by branching directly from that authoritative commit before selecting this batch.
- Root `reviewed-mrs.md` was re-read from the authoritative predecessor branch. No entry there supersedes the lower frontier or marks any of !5361-!5410 as previously reviewed.
- The historical `reviewed-mrs-automation-17571-17620.md` was re-read directly and still enumerates exactly 50 unique reviewed MR numbers from !17571 through !17620 with no omissions. That batch remains preserved and counted.
- Every selected corpus file `mr_5361.json` through `mr_5410.json` was fetched successfully from the pinned corpus commit before review, so there are no missing corpus holes in this batch.

## Evidence weighting

Merged master changes are primary evidence. Stable-branch cherry-picks are corroboration. Closed !5380, !5366, and !5361 are retained only as supersession or design-history evidence. Direct maintainer reasoning is weighted more heavily; useful discussion in this batch includes Gerald Combs, João Valverde, Jaap Keuter, Pascal Quantin, Jörg Mayer, Alexis La Goutte, and John Thacker.

Next frontier: MR !5360 is reserved for a metadata-only post-run probe and is not part of this ledger.
