# Wireshark MR review batch !25828–!25877

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Notebook base commit: `73e778de12439940391fc74ecf021327d9f8f7d1`

## Selection and prior-review accounting

Before selecting this batch, the reviewed-MR set was rebuilt from all available tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md`, the supplemental tracking under `reviewed-mrs-automation/`, and the per-run files in that directory. MR numbers were extracted and treated individually. In particular, broad prose summaries of previously reviewed numeric regions were not used as proof that every MR in those regions had been reviewed.

That exact accounting exposed a previously skipped block below the already-ledgered !25878-and-newer work. After subtracting every individually tracked reviewed MR from the corpus and sorting the remainder in descending MR-number order, the fifty highest-numbered previously unreviewed MRs were !25877 through !25828 inclusive.

The required historical !17571–!17620 batch remains preserved and counted as previously reviewed; this run does not replace or invalidate that ledger.

## Exact MRs reviewed

!25877, !25876, !25875, !25874, !25873, !25872, !25871, !25870, !25869, !25868,
!25867, !25866, !25865, !25864, !25863, !25862, !25861, !25860, !25859, !25858,
!25857, !25856, !25855, !25854, !25853, !25852, !25851, !25850, !25849, !25848,
!25847, !25846, !25845, !25844, !25843, !25842, !25841, !25840, !25839, !25838,
!25837, !25836, !25835, !25834, !25833, !25832, !25831, !25830, !25829, !25828.

Count: **50**.

## Review weighting and durable findings

Merged master MRs were treated as the strongest evidence. Stable-branch backports were used as corroboration of fixes already accepted on master. Open, closed-without-merge, reverted, and superseded work was deliberately down-weighted. Substantive review from established maintainers was weighted more heavily than routine approval metadata; changes authored or directly reasoned about by Guy Harris, John Thacker, Gerald Combs, Anders Broman, Pascal Quantin, Stig Bjørlykke, and other established maintainers were interpreted in that context.

Durable notebook additions from this batch:

- **!25877 — normalize representation units where semantic ownership changes.** Guy Harris's merged DCT2000 cleanup converts source character counts to record byte counts at the record-sizing boundary, with the comment-record and ASCII-hex packet cases handled explicitly. Added to `api-domain-conventions.md` with extremely high confidence because Guy authored and merged the change.
- **!25873 — consume aliased inputs before mutating an output parameter.** John Thacker's merged `nstime_delta()` fix demonstrates that an in-place-capable API must preserve input members until every calculation that reads them is complete. Added to `api-domain-conventions.md`.
- **!25834 — random-access wiretap reads must restore sequentially derived per-record context.** The merged 3GPP 32.423 fix snapshots session state by `data_offset` during the first pass and restores it during `seek_read`, eliminating dependence on whichever session happened to be parsed last. Added to `stateful-reassembly-conventions.md`.
- **!25841, corroborated by stable !25857 and follow-up !25862 — determine effects, quiesce consumers, then apply preference mutations.** The accepted Qt preference flow first computes the full redissection effect set, freezes the packet list when required, and only then applies state changes; !25862 shows that auxiliary panels contributing effect flags must be included in that first phase. Added to new `preference-state-conventions.md`.
- **!25845 — lexer case semantics and reserved-name validation must remain consistent.** John Thacker's merged display-filter change makes operators case-insensitive, updates registration-time reserved-name checks to the same semantics, and explicitly preserves case-sensitive escape introducers. Added to `dfilter-conventions.md`.
- **!25830 — pin unreleased CI tooling to an immutable revision.** Gerald Combs's merged CI change, approved/merged by John Thacker, consumes an upstream `aqtinstall` capability not yet on PyPI by pinning the Git dependency to an exact commit rather than a moving branch. Added to `ci-tooling-conventions.md`.

Strong corroborating evidence was retained without creating duplicate notebook rules. **!25876**, authored and merged by Guy Harris, reinforces the existing semantic-domain rule that offsets, sizes, and timestamp quantities known to be non-negative should use unsigned types. **!25844** and **!25837**, both merged reassembly hardening changes from John Thacker, reinforce checked/capped aggregate lengths and the requirement that overlap comparisons never extend past the allocated reassembly buffer. **!25864**, with stable backports **!25866/!25867**, shows that ASCII-hex conversion must process only complete byte pairs rather than reading a dangling nibble. **!25875** and **!25851** reinforce shared protocol decode helpers and persistence of request-side parameters needed for response decoding. **!25853** reinforces making nested protocol context explicit—current versus parent TLS session—rather than repeatedly inferring ambiguous state from a generic `packet_info`. **!25832/!25838** reinforce avoiding generic packet-value fetches for field types that do not actually consume that scalar representation. **!25868/!25870/!25871** provide accepted cross-version preference compatibility evidence for normalizing newer Qt font serialization before older Qt versions consume it.

Guy Harris's merged **!25869** was given high-authority weight but is principally a readability cleanup making comment/non-comment branches consistently oriented; it did not justify a broader convention beyond existing consistency/readability guidance. Other small merged fixes in the batch were reviewed but likewise not promoted when they were protocol-specific, release-note/documentation-only, image/resource-only, or already covered by stronger notebook rules.

Negative or unresolved evidence was deliberately down-weighted. **!25872** was still open in the corpus snapshot and therefore was not treated as accepted build/documentation policy. The revert proposals **!25859** and **!25860** were closed without becoming the authoritative outcome. **!25839** remained open with unresolved blocking discussion despite a successful pipeline; Stig Bjørlykke explicitly asked that it not be merged and identified multiple profile-state and cross-version regressions. It is useful review evidence that a green pipeline does not establish preference-migration correctness, but it was not promoted as accepted architecture.

## Notebook files updated in this run

- `api-domain-conventions.md`
- `stateful-reassembly-conventions.md`
- `preference-state-conventions.md`
- `dfilter-conventions.md`
- `ci-tooling-conventions.md`
- `reviewed-mrs-automation/reviewed-mrs-automation-25828-25877.md`

`reviewed-mrs.md` was not modified. The exact per-run ledger above is authoritative for this batch and should be included when constructing the reviewed-MR set for subsequent runs.
