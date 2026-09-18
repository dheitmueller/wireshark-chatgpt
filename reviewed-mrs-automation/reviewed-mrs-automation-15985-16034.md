# Automated MR review: !15985-!16034

Corpus commit: `74934ed18caaf0aac0fe5ae9f71952f38437be70`

Review direction: descending from newest unreviewed MRs toward older MRs.

## Exact reviewed set

!16034, !16033, !16032, !16031, !16030, !16029, !16028, !16027, !16026, !16025, !16024, !16023, !16022, !16021, !16020, !16019, !16018, !16017, !16016, !16015, !16014, !16013, !16012, !16011, !16010, !16009, !16008, !16007, !16006, !16005, !16004, !16003, !16002, !16001, !16000, !15999, !15998, !15997, !15996, !15995, !15994, !15993, !15992, !15991, !15990, !15989, !15988, !15987, !15986, !15985

Count: 50.

The previously reviewed !17571-!17620 batch remains part of the accumulated reviewed set. Selection was made from the accumulated per-run ledgers plus `reviewed-mrs.md`, not by assuming numeric ranges were complete.

## Durable findings / weighting

Merged MRs were weighted more heavily than closed, abandoned, or superseded work.

- !15990 (merged, Guy Harris): non-heuristic dissectors and heuristic dissectors have different return-value contracts. A heuristic dissector should return `bool`; a normal dissector should return 0 when it declines the packet and the number of bytes dissected when it accepts it. When adapting shared logic, keep the heuristic implementation boolean and wrap it for normal registration rather than returning TRUE/FALSE through the normal dissector ABI. This is strong, authoritative evidence because Guy authored and merged the correction.
- !16030 (merged, Pascal Quantin; merged by Anders Broman): broad conversion of plugin epan code to C99 types reinforces the ongoing project migration away from GLib integer/boolean aliases where native C99 types are appropriate.
- !16020 (merged, Anders Broman): `table_report_header` was converted to a boolean, another small corroborating example of using semantically accurate native types.
- !16010 (merged, John Thacker): BIER-over-MPLS support was added as a dedicated dissector and iterated in review, reinforcing protocol-layer separation and registration through the appropriate encapsulation mechanism.
- !16000 (closed): URL-only CCSDS fix was closed because it was submitted from the wrong branch and recreated as !16001. Treat !16000 as superseded submission-history evidence, not as architectural guidance.
- !16034 (merged, Gerald Combs): documentation was updated together with configurable automatic-profile-switching behavior, including the disabling value, reinforcing that user-visible configurable behavior should have its exact semantics documented.

## Notebook impact

The strongest durable rule in this batch, the heuristic-vs-normal dissector return contract from !15990, is already represented by existing notebook guidance around dissector contracts/registration; the C99-type migrations likewise reinforce existing guidance. No convention file was changed solely to duplicate those rules.

Next run must rebuild the reviewed set from all ledgers and `reviewed-mrs.md`. If the corpus remains contiguous below this batch, the next candidate is !15984; do not assume that without checking the actual corpus and accumulated reviewed set.