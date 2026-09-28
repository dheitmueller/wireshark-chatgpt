# Wireshark MR review !6561-!6610

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Reviewed exactly: !6610, !6609, !6608, !6607, !6606, !6605, !6604, !6603, !6602, !6601, !6600, !6599, !6598, !6597, !6596, !6595, !6594, !6593, !6592, !6591, !6590, !6589, !6588, !6587, !6586, !6585, !6584, !6583, !6582, !6581, !6580, !6579, !6578, !6577, !6576, !6575, !6574, !6573, !6572, !6571, !6570, !6569, !6568, !6567, !6566, !6565, !6564, !6563, !6562, !6561.

Outcomes: 49 merged; closed/unmerged: !6578.

Tracking consulted before selection:
- The full `reviewed-mrs-automation/` inventory at notebook commit `321144f1853f705268be2ab38145e64d03235b14`: 395 ordinary exact-range ledgers and 17 nonstandard gap/backfill/noncontiguous/exact-list/aggregate trackers.
- No ordinary exact-range ledger overlaps !6561-!6610.
- Every nonstandard tracker plus `reviewed-mrs.md` was checked for candidate references; none records a candidate as reviewed.
- The preceding exact ledger !6611-!6660 was revalidated as 50 unique reviewed MRs. Its only reference to !6610 is the explicit next-frontier probe, marked not reviewed.
- The preserved !17571-!17620 ledger was revalidated as exactly 50 unique MRs spanning the entire range and remains counted.

Selection result: the fifty highest-numbered previously unreviewed MRs available in the corpus are exactly !6610 through !6561.

Next frontier (not reviewed): !6560 (`dfilter: Fix use after free with references`, merged on master).
