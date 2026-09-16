# Automated Wireshark MR review: !17909–!17958

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Review direction: descending MR number, continuing backward from the previously recorded !17959–!18008 run. The existing review ledgers and `reviewed-mrs.md` were consulted before selection; the historical !17571–!17620 batch remains part of the already-reviewed set.

## Exact MRs reviewed

!17958, !17957, !17956, !17955, !17954, !17953, !17952, !17951, !17950, !17949,
!17948, !17947, !17946, !17945, !17944, !17943, !17942, !17941, !17940, !17939,
!17938, !17937, !17936, !17935, !17934, !17933, !17932, !17931, !17930, !17929,
!17928, !17927, !17926, !17925, !17924, !17923, !17922, !17921, !17920, !17919,
!17918, !17917, !17916, !17915, !17914, !17913, !17912, !17911, !17910, !17909.

Count: **50**.

## Review notes and weighting

Merged MRs were treated as stronger evidence than closed, abandoned, duplicate, or superseded submissions. Human review discussion was weighted above mechanical/system notes, with established maintainer feedback weighted most strongly.

### Durable / representative findings

- **!17958 (merged, release-4.2): POP state across dissection passes.** The backport stores mutable conversation state only during the first pass and copies the information needed for subsequent passes into per-packet data. This is a strong example of Wireshark's multi-pass dissection model: state that evolves while walking packets should not be allowed to produce different results merely because a packet is revisited. This corroborates existing notebook guidance on first-pass state and deterministic redissection.
- **!17917 (merged, John Thacker): ftype time multiplication.** The accepted fix separates seconds and nanoseconds carefully, uses checked multiplication for integer intermediates, carries nanoseconds into seconds, and for floating-point multiplication converts the full `nstime_t` to seconds before splitting the result back into integral/fractional parts. The MR explicitly notes the remaining extreme-value limitation and the likely need for 128-bit intermediates. This reinforces existing checked-arithmetic and representation-boundary guidance rather than creating a new convention.
- **!17925 / !17930 (merged successor vs. closed duplicate): GitLab CI plugin list.** !17925, authored by Gerald Combs, fixed the CMake list representation and merged. !17930 independently addressed the same problem but John Thacker closed it after noting that Gerald had already fixed it in !17925. Treat !17925 as implementation evidence and !17930 only as duplicate/supersession history.
- **!17909 (merged): Stratoshark profile auto-switch.** Adds the CloudTrail profile's automatic switch filter through profile settings. Straightforward accepted configuration work; no substantive reviewer correction.

The remaining MRs in the batch were scanned for state, purpose, changes, and substantive discussion. Their findings were either narrowly protocol/UI/build-specific or corroborated conventions already captured by stronger evidence, so no additional durable convention file was added in this run.

## Notebook changes

No convention file required modification. This ledger is the notebook update for the run and is intentionally stored under `reviewed-mrs-automation/`.

## Continuation

For the next run, rebuild the already-reviewed set from all tracking files again and select the fifty highest corpus MR numbers not present in that set. If there are no unexpected holes in higher-numbered tracking, the next descending candidates begin below !17909. Do not infer completeness solely from this numeric range; use the exact MR list above.
