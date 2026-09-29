# Wireshark MR review ledger 5561-5610

Corpus repository: dheitmueller/wireshark-corpus-mrs

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

Notebook base: 655f2db02cc02d4e2c2ab42a04882c6e1b8fee9f (automation/mr-review-5611-5660-authoritative)

Reviewed exactly 50 previously unreviewed merge requests in descending order:

!5610 !5609 !5608 !5607 !5606 !5605 !5604 !5603 !5602 !5601
!5600 !5599 !5598 !5597 !5596 !5595 !5594 !5593 !5592 !5591
!5590 !5589 !5588 !5587 !5586 !5585 !5584 !5583 !5582 !5581
!5580 !5579 !5578 !5577 !5576 !5575 !5574 !5573 !5572 !5571
!5570 !5569 !5568 !5567 !5566 !5565 !5564 !5563 !5562 !5561

Outcome count: 47 merged; 3 closed/unmerged (!5606, !5563, !5562).

Tracking reconciliation:
- The previous exact ledger reviewed !5611 through !5660 and explicitly marked MR 5610 as a metadata-only frontier probe, not a review.
- The review-tracking directory on the authoritative base contained 581 entries. Candidate membership for !5561-!5610 was checked against the irregular/exact-list/gap/backfill/noncontiguous/aggregate trackers, including reviewed-mrs-automation.md, and against root reviewed-mrs.md; no candidate was already recorded as reviewed.
- The ordinary exact-range history continues downward only through the preceding !5611-!5660 batch, so the current selection was still made by individual membership reconciliation rather than by assuming an entire numeric interval was untouched.
- reviewed-mrs-automation/reviewed-mrs-automation-17571-17620.md was revalidated as exactly 50 unique reviewed MR numbers with no omissions, preserving and counting that historical batch.

Evidence weighting:
- Merged master changes are preferred over release backports.
- Closed or superseded proposals are retained only for review-history or negative evidence.
- Later corrective MRs outweigh earlier merged implementations when they demonstrate that the earlier implementation was wrong.
- Maintainer-authored and maintainer-reviewed evidence is weighted accordingly; in this batch the strongest high-authority evidence includes Guy Harris's !5573, !5566, !5567, and review discussion on !5600.

Next frontier: MR 5560, "text_import: Use time format directly", exists at the same corpus commit, is merged on master, and was inspected only for metadata. It was not reviewed in this run.
