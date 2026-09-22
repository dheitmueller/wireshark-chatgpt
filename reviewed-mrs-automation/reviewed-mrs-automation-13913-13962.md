# Wireshark MR review automation ledger: !13913–!13962

- Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook starting commit: `605cda9c3664d35294c77edb153ca95eb78f90da`
- Selection method: reconciled the notebook's review tracking inventory, `reviewed-mrs.md`, the supplemental aggregate automation ledger, and the preceding exact per-run ledger. The prior exact ledger explicitly recorded !13962 as a frontier probe rather than a review. The corpus snapshot was unchanged, so exact already-reviewed-set subtraction selected the fifty highest remaining MR numbers below the prior frontier. The historical !17571–!17620 batch remains part of the reviewed set.
- Reviewed count: **50**
- Status mix: **47 merged**, **3 closed/unmerged** (!13930, !13927, !13926). The closed MRs were down-weighted; !13927 was superseded by merged !13951 and !13926 by merged !13928.

## Exact reviewed MR numbers

!13962, !13961, !13960, !13959, !13958, !13957, !13956, !13955, !13954, !13953,
!13952, !13951, !13950, !13949, !13948, !13947, !13946, !13945, !13944, !13943,
!13942, !13941, !13940, !13939, !13938, !13937, !13936, !13935, !13934, !13933,
!13932, !13931, !13930, !13929, !13928, !13927, !13926, !13925, !13924, !13923,
!13922, !13921, !13920, !13919, !13918, !13917, !13916, !13915, !13914, !13913.

## Durable notebook updates

- `input-resource-limit-conventions.md`: merged master !13939 distinguishes a protocol-valid logical size from speculative memory commitment; cap initial RTMPT allocation and grow as bytes arrive rather than imposing an arbitrary protocol-size ceiling. Merged master !13933 plus stable !13934–!13936 establish that iteration/depth budgets must fail exactly at exhaustion and require valid-input coverage as well as pathological-input coverage.
- `single-pass-analysis-conventions.md`: merged master !13940, with direct John Thacker review of the one-pass question, establishes that early protocol phases such as QUIC 0-RTT must dispatch from information actually available at that phase (client-offered ALPN), with separate early/final state when later negotiation can differ.
- `fuzz-harness-conventions.md`: merged master !13950 establishes that a fuzz finding should first be reproduced with the exact fuzz target/entry point; wrapping the same bytes in a generic capture can select a different dissector/path and legitimately fail to reproduce.
- `tap-data-contract-conventions.md`: merged master !13931, with John Thacker review of reassembly and subdissector semantics, establishes that tap emission belongs at the layer representing the logical event being counted, not inside an optional payload-only path. Stable !13948/!13949 corroborate the result.
- `output-failure-state-conventions.md`: merged master !13924 and release-4.2 !13925 establish that caller-visible output failure sentinels must be initialized before any early-return path that can fail.

Other retained evidence includes !13957's RF4CE heuristic fix (do not validate encrypted bytes as if they were plaintext discriminators), !13954's staged clang-tidy rollout (clean/suppress the baseline before ratcheting a new check to error), !13929's real macOS DMG validation of a package-path fix, and !13914/!13918/!13919's WinSparkle lifecycle fix preserving the Qt event loop until the updater reaches the actual shutdown callback. These corroborate existing notebook directions and were not duplicated into new rules in this run.

Closed/unmerged !13930 was treated only as negative/contextual evidence because the author still had unresolved protocol-layout/endian and reassembly questions. Closed !13927 and !13926 were superseded by later merged submissions and therefore carry less weight than their accepted successors.

## Notebook commits from this run

- `5f392bcb7a6436303ff4b984821754204733d548` — add RTMPT resource-limit lessons.
- `539cb06d74a8702703424d3c3b464e39bc1656de` — record early-data single-pass dispatch rule.
- `f52e9418ef3674e15293b7c18405146fe4dbe1e6` — add fuzz reproduction entry-point rule.
- `90ef8e8643154dc89b20d8fd7f2af76c2a290be8` — record logical-event tap emission rule.
- `bc2d11c64e05582ae799dc5c010c04ddefc9bdf3` — record output failure-state initialization rule.

## Frontier check

`mr_13912.json` exists at the same corpus commit and was fetched only to verify the next frontier. **!13912 was not reviewed or counted in this run.** It remains eligible for the next run unless a newer previously-unreviewed MR appears in a later corpus snapshot.