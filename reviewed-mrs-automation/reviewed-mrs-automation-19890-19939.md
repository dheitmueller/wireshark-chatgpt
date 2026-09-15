# Automated Wireshark MR review: !19890-!19939

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Direction: newest to oldest. Selection was made after accounting for the existing per-run ledgers and `reviewed-mrs.md`, including the historical !17571-!17620 batch and the previously backfilled !22966. The corpus revision was unchanged from the preceding run, so no newly populated higher-numbered hole displaced this frontier.

Exactly 50 MRs reviewed:

!19939, !19938, !19937, !19936, !19935, !19934, !19933, !19932, !19931, !19930,
!19929, !19928, !19927, !19926, !19925, !19924, !19923, !19922, !19921, !19920,
!19919, !19918, !19917, !19916, !19915, !19914, !19913, !19912, !19911, !19910,
!19909, !19908, !19907, !19906, !19905, !19904, !19903, !19902, !19901, !19900,
!19899, !19898, !19897, !19896, !19895, !19894, !19893, !19892, !19891, !19890.

## Durable findings

- !19937 (merged master, authored and merged by Guy Harris): serialized/capture-file size must not be inferred from `sizeof(struct)` when compiler/ABI padding is not part of the external representation. Added `wire-layout-conventions.md`.
- !19938 (merged master, authored and merged by John Thacker): parser-owned strings/lists should use an explicit wmem allocator whose lifetime matches the retained parser state. This strongly corroborates the existing allocator-scope rules, including choosing the narrowest sufficient lifetime and avoiding repeated broader-scope allocations where a shorter-lived parse/copy design is possible.
- !19939 (merged master): review explicitly removed commented-out dead code rather than retaining disabled implementation fragments; useful hygiene evidence but not promoted to a separate convention.
- !19890 (merged master, authored by John Thacker): retrieved values must be normalized with the same bitmask declared by the field definition when downstream logic assumes the masked domain; useful protocol-specific correctness evidence but not broad enough to justify a new notebook rule by itself.

Merged changes and direct senior-maintainer guidance were weighted above abandoned, superseded, or purely mechanical changes. MRs without a durable cross-cutting lesson remain represented by this exact-run ledger rather than generating low-confidence notebook rules.