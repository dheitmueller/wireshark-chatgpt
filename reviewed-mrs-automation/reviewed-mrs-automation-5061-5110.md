# Wireshark MR review ledger 5061-5110

Corpus repository: dheitmueller/wireshark-corpus-mrs
Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054
Notebook base: 0d745c2ca76db18d2769876cdc57ba6563ba2ae7

Reviewed exactly 50 previously unreviewed merge requests in descending order:

!5110 !5109 !5108 !5107 !5106 !5105 !5104 !5103 !5102 !5101
!5100 !5099 !5098 !5097 !5096 !5095 !5094 !5093 !5092 !5091
!5090 !5089 !5088 !5087 !5086 !5085 !5084 !5083 !5082 !5081
!5080 !5079 !5078 !5077 !5076 !5075 !5074 !5073 !5072 !5071
!5070 !5069 !5068 !5067 !5066 !5065 !5064 !5063 !5062 !5061

Outcome count: 49 merged; 1 closed/unmerged (!5094).

Tracking reconciliation:
- Continued from authoritative notebook state `0d745c2ca76db18d2769876cdc57ba6563ba2ae7`. Its exact !5111-!5160 ledger records !5110 only as a metadata-only frontier probe, not as reviewed.
- That predecessor state had already reconciled the complete review-tracking tree, including ordinary exact-range ledgers, irregular/exact-list/gap/backfill/noncontiguous/aggregate trackers, auxiliary review notes, and root `reviewed-mrs.md`.
- This run re-read the predecessor ledger and root `reviewed-mrs.md`, then performed candidate-specific notebook searches for every MR !5061-!5110. None was found in review-tracking material as previously reviewed.
- Re-read `reviewed-mrs-automation/reviewed-mrs-automation-17571-17620.md`; it still explicitly records exactly 50 unique reviewed MRs, minimum !17571 and maximum !17620, with no omissions. That historical batch remains preserved and counted.
- The contiguous result is therefore exact candidate-membership subtraction from the accumulated reviewed set, not an assumption that the interval was wholly unreviewed.
- Fetched and reviewed all 50 pinned corpus records `mr_5061.json` through `mr_5110.json`, including captured discussions and diffs, at the corpus commit above.

Evidence weighting:
- Merged master changes are primary evidence. Maintained-branch backports corroborate master origins when both are present.
- Closed !5094 is lower-weight design/review history only and is not treated as accepted TCP architecture.
- Transitional merged changes are explicitly marked as superseded where a later accepted change gives stronger guidance.

High-authority review:
- Guy Harris-authored merged !5063 is exceptionally strong evidence for the public boundary of third-party plugin examples; !5064 corroborates it on release-3.6.
- Guy-authored !5061 is retained with an explicit supersession note: its unconditional `config.h` rule applies to Wireshark-owned build contexts, while later !5063 intentionally removes Wireshark's private `config.h` from the external plugin example.
- Jaap Keuter's direct review in merged !5079 strongly supports `proto_tree_add_bitmask()` for packed flag fields.
- Anders Broman's direct review in merged !5110 strongly supports keeping a dissector generator in-tree with its generated dissectors.
- Uli Heilmeier's review in merged !5109 gives concrete submission-scope and commit-message guidance.
- John Thacker's discussion in closed !5094 is useful wraparound/design caution but is down-weighted because the proposal was abandoned.

Continuity:
- MR !5060 (`We cannot use HAVE_CONFIG_H`, release-3.6, Guy Harris) exists at the same corpus commit. Only top-level metadata was inspected to establish the next descending frontier; it is not counted as reviewed.
- The corpus is not exhausted.
