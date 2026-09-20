# Wireshark MR review automation: !16049–!16098

Corpus commit: `130eea3c8a60902c0fc98c9e1130e73bafc6f815`

## Selection and duplicate-avoidance

This run selected the batch by enumerating the complete recursive Git tree for the corpus commit, reconstructing the already-reviewed set from all available tracking in `dheitmueller/wireshark-chatgpt` (the per-run files under `reviewed-mrs-automation/`, `reviewed-mrs.md` where applicable, and the supplemental automation tracker), subtracting that exact reviewed set from the corpus MR set, sorting the remainder numerically descending, and taking the first 50. No numeric interval was assumed reviewed merely because neighboring MRs appeared in a ledger. The historical !17571–!17620 batch remains explicitly preserved and counted as already reviewed.

The immediately preceding tracked batch is !16099–!16148, and a lower tracked batch is !15985–!16034; exact set reconciliation therefore identified the highest 50 unreviewed corpus MRs as !16098 through !16049. The corpus continues below this batch (`mr_16048.json` exists at the same corpus commit).

## Exact MRs reviewed

!16098, !16097, !16096, !16095, !16094, !16093, !16092, !16091, !16090, !16089,
!16088, !16087, !16086, !16085, !16084, !16083, !16082, !16081, !16080, !16079,
!16078, !16077, !16076, !16075, !16074, !16073, !16072, !16071, !16070, !16069,
!16068, !16067, !16066, !16065, !16064, !16063, !16062, !16061, !16060, !16059,
!16058, !16057, !16056, !16055, !16054, !16053, !16052, !16051, !16050, !16049.

Count: **50**.

## Weighting and review notes

Merged changes were treated as accepted project evidence; this batch is dominated by merged work. Maintainer-authored/merged changes from John Thacker and Guy Harris were given particularly high weight, and release-branch cherry-picks were used mainly as corroboration of master behavior rather than as independent conventions.

### Durable findings promoted to the notebook

- **!16095 — vendored-generator synchronization is a semantic port.** John Thacker imports upstream PIDL `int64` support but explicitly skips an adjacent Samba change that would switch generated calls to `dissect_ndr_int64()`, because Wireshark intentionally uses its local NDR API differently while exposing signed `FT_INT64` semantics. Upstream ancestry does not make a change automatically correct for Wireshark; local generated-code contracts must be reviewed and deliberate divergences documented. Added to `generated-code-conventions.md` in commit `cfa0c3833dec32fea15ea5c245338ab59eaead66`.

- **!16084, !16083, !16089, and !16096 — parser control state needs resource bounds too.** The PIDL sync adds explicit NDR recursion-depth checking, prevents recursive type-list traversal from looping forever, and consumes one-shot switch/array tokens instead of allowing them to accumulate until internal limits are exhausted. These are distinct from simple byte/count allocation bounds: malformed input can exhaust stack or transient parser bookkeeping. Added to `input-resource-limit-conventions.md` in commit `5819e58d43a2c42bf89acd5deb966e9d3bc08fe4`.

- **!16081 (with !16080) — stricter warnings are semantic review signals.** Enabling Perl warnings in PIDL exposed real correctness problems, including hexadecimal range bounds represented as strings being treated numerically as zero, plus precedence/initialization issues. Warning-enablement work should classify diagnostics and fix semantics rather than merely suppress or cast them away. Added to `checker-tooling-conventions.md` in commit `d2e6996f5b03893b54b5fb13f59af966f7874e7c`.

### Strong corroborating evidence retained without duplicate rules

- **!16091** moves PIDL compression-state cleanup into the owning talloc lifecycle so failure paths cannot omit release; this corroborates existing ownership/lifetime rules.
- **!16077** adds bound checks for arrays-of-arrays after Honggfuzz findings and enriches generated error locations; corroborates hostile-input boundary checking and actionable generated diagnostics.
- **!16073** extends checker coverage for duplicate `value_string` entries; generated duplicates can require changing the generator/template rather than hand-editing output, reinforcing existing checker and generated-code guidance.
- **!16071/!16070** continue upstream PIDL portability/Python/C99 cleanup. Useful accepted evidence, but not novel enough to justify separate rules.
- **!16065** fixes an array overrun found by GCC static analysis aided by allocation-size annotations; reinforces the value of static analysis and accurate allocation contracts.
- **!16063** fixes Qt dark-mode detection by treating `QStyleHints::colorScheme()`'s `Unknown` as genuinely unknown and falling back to observed colors instead of mapping it implicitly to Light; corroborates defensive platform-GUI handling.
- **!16060** adds the XML source and regeneration instructions for generated dissectors and regenerates their output; directly reinforces existing generated-source provenance and reproducible-regeneration rules.
- **!16058** handles nested RADIUS `$INCLUDE` path context correctly, including restoring the enclosing directory after returning from an included file.
- **!16057** changes masked Matter flag fields from integer fields to `FT_BOOLEAN` with the correct width/masks, reinforcing that registered field type should match the field's semantic domain.
- **!16056** adds standalone LeakSanitizer support. Guy Harris suggested possible future consolidation of sanitizer configuration but explicitly accepted the current implementation because sanitizer link-time behavior differs; this is useful evidence against forcing premature abstraction across superficially similar tool modes.
- **!16054**, authored and merged by Guy Harris, factors compression-specific `fill_out_buffer()` paths into focused routines and uses an explicit switch for the compression mode. High-authority accepted control-flow cleanup, but sufficiently local that no separate universal rule was added.
- **!16053/!16052/!16050** carry the same RADIUS include-open failure fix on maintained branches and master: save the prior input stream and restore it when a nested include cannot be opened. This strongly corroborates existing transactional/partial-initialization cleanup guidance: a failed speculative context switch must leave the previous usable context intact.
- **!16051**, authored and merged by Guy Harris, renames `gz_head()` to `check_for_compression()` after its scope expanded beyond gzip and documents the recognized formats. This is strong naming/readability evidence: names should describe the actual semantic scope after functionality grows.
- **!16049**, authored and merged by Guy Harris, annotates long-distance `#else`/`#endif` directives with their matching conditions. This is accepted readability guidance for complex conditional compilation, but was not promoted as a standalone project-wide rule from this single localized cleanup.

## Notebook commits made by this run

- `5819e58d43a2c42bf89acd5deb966e9d3bc08fe4` — extend hostile-input resource guidance to recursion and transient parser state.
- `d2e6996f5b03893b54b5fb13f59af966f7874e7c` — record warning-enablement as semantic bug-discovery/review work.
- `cfa0c3833dec32fea15ea5c245338ab59eaead66` — record semantic review requirements for vendored upstream generator synchronization.

This ledger records the exact 50 MRs reviewed in this run and the exact corpus commit used.