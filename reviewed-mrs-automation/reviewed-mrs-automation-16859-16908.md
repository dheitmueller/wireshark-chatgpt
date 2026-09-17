# Automated Wireshark MR review: !16859-!16908

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Direction: descending from newest toward older MRs. Existing per-run ledgers and `reviewed-mrs.md` were consulted before selection. The historical !17571-!17620 batch remains part of the already-reviewed set. Selection was based on individual MR membership, not assumed numeric coverage.

## Exact reviewed set

!16908, !16907, !16906, !16905, !16904, !16903, !16902, !16901, !16900, !16899,
!16898, !16897, !16896, !16895, !16894, !16893, !16892, !16891, !16890, !16889,
!16888, !16887, !16886, !16885, !16884, !16883, !16882, !16881, !16880, !16879,
!16878, !16877, !16876, !16875, !16874, !16873, !16872, !16871, !16870, !16869,
!16868, !16867, !16866, !16865, !16864, !16863, !16862, !16861, !16860, !16859.

Count: 50.

## Review notes

Merged master changes were weighted most strongly; release backports, automatic updates, cosmetic cleanups, abandoned drafts, and superseded submissions were counted but given less independent architectural weight.

- **!16908 — O-RAN FH CUS SE6/SE10 fixes (merged, master).** Martin Mathieson explicitly notes that SE6 still needs further work to display the correct PRBs for each bundle. Useful reminder that an accepted incremental protocol fix may deliberately leave a known limitation documented rather than expanding scope.
- **!16907 — Clang Analyzer dead-assignment cleanup (merged).** Static-analysis-driven cleanup; reinforces existing analyzer/pre-submit guidance without adding a new convention.
- **!16906 — BTLE format truncation fix (merged).** Corrects maximum-string sizing by accounting for all literal separator characters; ordinary defensive formatting fix.
- **!16904 — O-RAN FH CUS ST4 command types (merged, master).** Protocol expansion marked untested; useful implementation history but weaker testing exemplar than changes carrying captures/tests.
- **!16893 — O-RAN FH CUS exponent initialization (merged, master).** Martin initializes a value even though protocol control flow should only consume it after it has been set. This is useful corroboration for keeping initialized storage distinct from assumptions about valid protocol state; initialization can silence analyzer concerns but does not replace semantic validation.
- **!16885 — `false` is not `NULL` (merged, master; John Thacker).** Replaces uses of boolean `false` in pointer contexts with `NULL`. The change is motivated by C23, where `false` has boolean type rather than being an integer-zero macro suitable for legacy null-pointer conversion. Durable portability lesson, but it fits existing type-correctness/compiler-forward-compatibility guidance.
- **!16880 — tshark option-error reporting (merged, master; John Thacker).** Keeps `ws_opterr` disabled across preliminary `ws_getopt_long()` passes and leaves actual error reporting to the final pass. Good example of multi-pass parsing assigning diagnostics to exactly one authoritative phase rather than allowing helper/default diagnostics to duplicate application-level reporting.
- **!16864 — ISO15765 formatting cleanup (merged).** Style-only consistency work; low independent weight.
- **!16861 — TECMP 1.9 update (merged).** Specification-driven enumeration/name update; counted, but no new general convention extracted.

## Durable conclusions

No notebook convention file required modification in this run. The strongest findings corroborate rules already represented in the notebook: use type-correct sentinel values rather than relying on historical implicit conversions; initialize storage defensively while separately validating protocol state; keep diagnostics owned by one parsing layer/phase; treat static-analysis warnings as prompts to verify semantics rather than merely suppressing them; and permit tightly scoped incremental protocol work when remaining limitations are made explicit.

## Continuation

Rebuild the reviewed set from all ledgers before the next run. If no higher-numbered hole is discovered, the next descending candidate is !16858.
