# Automated Wireshark MR review: !18809–!18858

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Review direction: descending from newest toward older MRs. This run consulted the available per-run files in `reviewed-mrs-automation/` and `reviewed-mrs.md`, preserving and counting the historical !17571–!17620 batch. Selection is based on individual MR membership, not inferred range coverage. The fifty highest-numbered corpus MRs not already represented in the tracking set were selected.

## Exact reviewed set

!18858, !18857, !18856, !18855, !18854, !18853, !18852, !18851, !18850, !18849,
!18848, !18847, !18846, !18845, !18844, !18843, !18842, !18841, !18840, !18839,
!18838, !18837, !18836, !18835, !18834, !18833, !18832, !18831, !18830, !18829,
!18828, !18827, !18826, !18825, !18824, !18823, !18822, !18821, !18820, !18819,
!18818, !18817, !18816, !18815, !18814, !18813, !18812, !18811, !18810, !18809.

Count: **50**.

## Durable findings

- !18858, !18857, !18856, !18855 and !18850 (merged): follow-up repairs after !18401 restore NULL checks and reset UAT/hash-table state to avoid dangling pointers. This is a concentrated corroboration of existing configuration-lifecycle guidance: replacing access patterns or refactoring ownership-bearing configuration code requires an explicit audit of teardown/reset behavior and nullable state.
- !18851 (merged): a Coverity-reported Qt null dereference is fixed with a defensive check on a parent-chain traversal value and was explicitly retested with Coverity. This corroborates the notebook's static-analysis workflow rather than adding a new rule.
- !18849 (merged, John Thacker): Rawshark is unsupported on big-endian systems, but `pytest.skip()` cannot be used at class scope; the accepted test uses the class-level skip decorator. Useful test-framework correctness, but not a Wireshark-specific convention.
- !18841 (merged, John Thacker): JPEG parsing accommodates a known non-conformant EXIF producer only through bounded plausibility checks while retaining awareness that the producer is broken. This is useful corroboration for conservative malformed-input handling rather than permission to invent protocol meaning from insufficient evidence.
- !18831 (closed/superseded): SocketCAN discussion is substantial but the MR did not merge and was later superseded by !18994, which is already represented in the reviewed set. The discussion strongly emphasizes that a heuristic lacking enough information to distinguish protocol variants can silently mislabel traffic; proposals included strict/loose parsing modes plus expert information for guessed cases. Because the accepted successor has already been reviewed, this MR is weighted as historical design context rather than a new implementation exemplar.
- !18820 (merged, John Thacker): static GLib dependency handling trusted pkg-config when available and selected PCRE vs PCRE2 based on the GLib version otherwise. Later OSS-Fuzz discussion exposed a static-linkage regression and led to merged follow-up !19005, already reviewed in the !18959–!19008 batch. Treat the pair as evidence that static-link dependency changes must be validated in the actual static consumer environment, but no new notebook rule is added here because the accepted corrective follow-up is already tracked.
- !18816 (merged): GNSS adds multi-band EGNOS file support and basic SBAS L5 dissection. The implementation was accepted after several updates but adds no general convention beyond existing protocol/parser guidance.
- !18815 (merged, Jaap Keuter): PTP updates a field name from the draft terminology to the final standard while retaining the old field hidden for display-filter compatibility. Alexis La Goutte requested an explanatory source comment for the hidden compatibility item before approval. This strongly corroborates existing filter-compatibility guidance: when a corrected standard name requires a new field abbreviation, preserve a legacy alias where practical and document why the otherwise surprising hidden item exists.
- !18814 and !18811 (merged, John Thacker): live/compressed capture handling treats EOF as temporary state while a capture is still growing, including stopping initial pcapng-block reads at the temporary EOF and clearing EOF before final/tail reads so stream footers can be consumed. Useful wiretap state-machine evidence, but too specialized for a new general convention.
- !18818 and !18817 were closed/unmerged and weighted below merged work; !18809 was superseded by merged !18812. !18852–!18854 are automatic updates and carry little reusable review evidence.

## Notebook impact

No convention file required modification in this run. The strongest durable lessons independently corroborate conventions already present in the notebook: lifecycle/reset ownership, conservative heuristic/malformed-input handling, static-analysis validation, filter compatibility, and weighting merged successors over abandoned/superseded attempts. Avoiding duplicate prose keeps the notebook concise.

## Continuation

Future runs must rebuild the reviewed set from individual entries. Subject to any higher-numbered holes discovered in tracking, the descending frontier after this run is below !18809.
