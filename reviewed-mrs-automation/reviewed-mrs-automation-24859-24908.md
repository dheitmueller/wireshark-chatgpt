# Wireshark MR review batch !24859–!24908

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Notebook base commit: `a9aacacb8b21250208f911230cfb13795f8da1f0`

## Selection and prior-review accounting

Before selecting this batch, the review set was rebuilt from all available tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md`, the supplemental `reviewed-mrs-automation/reviewed-mrs-automation.md` ledger, and all per-run files in `reviewed-mrs-automation/`. Their union establishes a continuous already-reviewed span of !24909–!26393, while the required historical !17571–!17620 batch remains explicitly preserved and counted. Selective entries were treated individually; no numeric range was considered reviewed merely because some members appeared in a ledger.

After subtracting that exact reviewed set from the corpus and sorting remaining MRs in descending MR-number order, the fifty highest-numbered previously unreviewed MRs are the batch below.

## Exact MRs reviewed

!24908, !24907, !24906, !24905, !24904, !24903, !24902, !24901, !24900, !24899,
!24898, !24897, !24896, !24895, !24894, !24893, !24892, !24891, !24890, !24889,
!24888, !24887, !24886, !24885, !24884, !24883, !24882, !24881, !24880, !24879,
!24878, !24877, !24876, !24875, !24874, !24873, !24872, !24871, !24870, !24869,
!24868, !24867, !24866, !24865, !24864, !24863, !24862, !24861, !24860, !24859.

Count: **50**.

## Review weighting and durable findings

Merged MRs were treated as accepted evidence and given substantially more weight than open, draft, abandoned, or superseded work. Maintainer rationale was weighted by authority; in particular, Guy Harris's explanation in merged !24883 was treated as especially strong evidence.

Durable notebook additions from this batch:

- **!24883 — platform applicability, not compiler behavior, should drive conditional compilation.** John Thacker noted that the dumpcap helper is unused on every Windows implementation, while GCC/Clang merely expose that fact through warnings. Guy Harris confirmed the architectural reason: capture pipes are read differently on UN*X and Windows. Added to `portability-conventions.md`.
- **!24870 — retain `nstime_t` internally and convert only at the required interface.** John Thacker replaced 32-bit epoch-second/fraction storage in TCP/RLC graph taps, eliminating Y2038/Y2106 narrowing. Added to `portability-conventions.md`.
- **!24867 — stable branches need CI for their own supported legacy variants.** The release-4.4 Qt 5 job is deliberately triggered for Qt changes because master-oriented coverage does not prove compatibility with the older branch's supported configuration. Added to `portability-conventions.md`.
- **!24864 and !24885 — gate invalid re-entry at the event source.** The accepted Lua debugger design prevents packet dissection and asynchronous tap drawing while paused, allowing lower-level re-entrant special cases to be removed. Added to `reentrancy-conventions.md`.

Strong corroboration that was not duplicated in the notebook includes !24905 and !24869 on bounded parser arithmetic/subset TVBs, !24896 on keeping malformed nested-element failure local to its bounded region, !24873 on synchronizing debugger shared state, !24871/!24876/!24900 on ownership cleanup, and !24860/!24877 on measured source-checker performance improvements.

Open/draft work was deliberately down-weighted: !24908 remains an unresolved WSLua API proposal; !24904 remains a draft TCP Follow change; and !24865 remains an open draft with a failed pipeline and changed approval state. Their current designs were therefore not promoted as accepted conventions.

## Notebook files added in this run

- `portability-conventions.md`
- `reentrancy-conventions.md`
- `reviewed-mrs-automation/reviewed-mrs-automation-24859-24908.md`

`reviewed-mrs.md` was not modified; the exact per-run ledger above is authoritative for this batch.
