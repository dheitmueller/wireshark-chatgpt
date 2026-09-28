# Reviewed MRs automation ledger: !6311–!6360

- Corpus: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook base before this run: `7b1d84a17d77b59f2ae11d068d23701395fc736f`
- Direction: descending from the newest previously unreviewed MR toward older MRs.
- Reviewed in this run: exactly 50 MRs.
- Outcomes: 46 merged; 4 closed/unmerged (!6358, !6356, !6352, !6349).
- Historical batch !17571–!17620 was revalidated as 50 unique reviewed MRs and remains counted.

## Tracking reconciliation

At the notebook base used for selection, `reviewed-mrs-automation/` contained 400 ordinary exact-range ledgers and 17 irregular/gap/backfill/noncontiguous/exact-list/aggregate tracking files. No ordinary exact-range ledger overlapped !6311–!6360. All 17 irregular/aggregate trackers plus `reviewed-mrs.md` were checked for candidate MR numbers and contained none. The prior run's mention of !6360 was only a frontier probe and did not count as a review.

## Exact reviewed set

!6360, !6359, !6358, !6357, !6356, !6355, !6354, !6353, !6352, !6351,
!6350, !6349, !6348, !6347, !6346, !6345, !6344, !6343, !6342, !6341,
!6340, !6339, !6338, !6337, !6336, !6335, !6334, !6333, !6332, !6331,
!6330, !6329, !6328, !6327, !6326, !6325, !6324, !6323, !6322, !6321,
!6320, !6319, !6318, !6317, !6316, !6315, !6314, !6313, !6312, !6311.

## Closed/unmerged MRs

- !6358 — superseded stable-branch form of the timestamp-validity fix; Guy Harris supplied the release-3.4 form in !6360.
- !6356 — packet-count handling proposal later overtaken by other capture-block work; useful only as lower-weight discussion evidence.
- !6352 — broad SSH/SFTP reassembly draft; later closed after the issue was considered solved by other work.
- !6349 — draft TLS layer-number workaround; explicitly superseded by later TCP-layer reassembly work (!6567, !6666 and related fixes), which carries higher architectural weight.
