# Automated MR review: non-contiguous batch ending at !15899

Corpus commit: `130eea3c8a60902c0fc98c9e1130e73bafc6f815`

Review direction: descending from the highest-numbered previously unreviewed MRs toward older MRs.

## Selection method

The already-reviewed set was rebuilt from the available `reviewed-mrs-automation/` per-run ledgers plus `reviewed-mrs.md`; the historical !17571-!17620 batch remains explicitly preserved and counted. Selection was by exact MR-number set subtraction against the current corpus tree, not by assuming a numeric range was reviewed merely because neighboring MRs appeared in a ledger.

This run is deliberately non-contiguous. Existing ledgers already record !16034-!15985 and !15984-!15935. The current corpus contains MRs that had been absent from an earlier corpus snapshot, so the fifty highest-numbered unreviewed MRs are fourteen entries immediately above that already-reviewed block and thirty-six immediately below it.

## Exact reviewed set

!16048, !16047, !16046, !16045, !16044, !16043, !16042, !16041, !16040, !16039, !16038, !16037, !16036, !16035, !15934, !15933, !15932, !15931, !15930, !15929, !15928, !15927, !15926, !15925, !15924, !15923, !15922, !15921, !15920, !15919, !15918, !15917, !15916, !15915, !15914, !15913, !15912, !15911, !15910, !15909, !15908, !15907, !15906, !15905, !15904, !15903, !15902, !15901, !15900, !15899

Count: 50.

All except !15919 were merged. !15919 was closed/unmerged and was therefore down-weighted as precedent.

## Durable findings / weighting

Merged master work and direct maintainer guidance were weighted most heavily. Stable-branch backports, packaging/release bookkeeping, spelling-only changes, automatic data updates, and the closed !15919 submission were reviewed but given little or no independent architectural weight.

- **!15925 — merged master; strong memory/compiler-contract rule.** `G_GNUC_MALLOC` was removed from `wmem_realloc()` and `wmem_memdup()` because GCC's malloc-like attribute carries aliasing/object-reachability semantics that those functions do not guarantee. Compiler allocation attributes are optimizer contracts, not descriptive labels. This was promoted to `memory-management-conventions.md`.
- **!15927 — merged master; John Thacker authored and merged.** The dumpcap sync command path must continue reading framed IPC records when an intermediate log message arrives and complete only on the protocol-defined success/failure response (or a real transport failure). Noisy logging previously caused early return, missing interfaces, or crashes. This was promoted to `capture-diagnostic-conventions.md` together with an explicit noisy/interleaving test rule.
- **!16039 — merged master; substantial Pascal Quantin review.** Review required the Matter dissector to preserve normal dissector return semantics (do not accidentally return 0/reject after recognizing the packet), use expert information rather than `ws_warning()` for packet-visible malformed conditions, expose useful filter fields, and organize the reusable Matter TLV parser with the real Matter protocol rather than inventing a synthetic protocol/source-file split. The source-file point corroborates the existing `dissector-conventions.md` rule that closely related dissectors may share one C file; no duplicate rule was added.
- **!16048 — merged master.** JSON number fields changed from `FT_STRING` to `FT_DOUBLE`, and tests changed from quoted-string comparisons to numeric display-filter comparisons. This strongly corroborates the existing field-semantic rule that filterable fields should represent the protocol concept's native semantic value rather than a parser/display representation.
- **!16047 — merged master, authored and merged by Guy Harris.** Wiretap defines one semantic `USE_ZLIB_OR_ZLIBNG` switch and uses it instead of repeatedly spelling alternative implementation feature tests. This is high-authority maintainability evidence for deriving a semantic capability once, though the change is local enough that no additional notebook rule was needed.
- **!16045 — merged master, authored and merged by Guy Harris.** Related decompression contexts were grouped together in `wtap_reader`, with an explicit note that type-specific contexts may eventually merit a union. High-authority structural cleanup, but too local to justify a separate architecture rule.
- **!16037 — merged master.** A broad removal of obsolete `= -1` initialization for protocol/hf/expert registration variables prompted Stig Bjørlykke to suggest teaching `checkhf.pl` to detect the old form. This corroborates the existing checker-tooling principle that stable mechanical repository invariants should become executable project policy rather than recurring manual-review work.
- **!15924 — merged master.** HTTP/2 avoids making `multipart/mixed` a sticky stream-wide dissector when 5G traffic can interleave multipart and JSON content on the same stream. This is useful state-scope evidence: cache/persist a derived dissector choice only when the protocol guarantees that lifetime.
- **!15914 — merged master.** A 45-file cleanup follows `README.dissector` guidance to use `col_add_str()` rather than `col_add_fstr(..., "%s", ...)` when no formatting is actually required; accepted API-style corroboration rather than a new rule.
- **!15907 — merged master.** `check_tfs.py` gained usage counting for shared true/false strings and removed unused opposite-order variants, reinforcing that repository tooling can expose dead shared definitions and consolidation opportunities.
- **!15903 — merged master; first contribution, merged by John Thacker.** Adding SNI-derived address resolution required updating console help and man-page documentation as well as the GUI/configuration path. John also preserved semantic grouping of dependent name-resolution flag letters rather than alphabetizing them mechanically. This corroborates existing configuration/documentation consistency guidance.
- **!15919 — closed/unmerged.** The submission was unrelated/noisy and provides no accepted Wireshark engineering precedent; it is counted as reviewed but deliberately not mined for conventions.

Other merged MRs in the batch were scanned for diffs/discussion and were primarily focused protocol corrections, Qt/UI consistency fixes, packaging/CI updates, stable release-note backports, C99-type conversions already covered by the notebook, automatic data/translation updates, or small diagnostic/display corrections. They did not add sufficiently distinct durable guidance to justify duplicating existing notebook rules.

## Notebook updates made by this run

- `memory-management-conventions.md`: added the rule that malloc-like compiler attributes must satisfy their aliasing/object semantics and must not be applied mechanically to realloc-like or otherwise nonconforming allocation helpers. Evidence: !15925.
- `capture-diagnostic-conventions.md`: added the rule that asynchronous log/status frames in command IPC are intermediate records, not terminal command responses; continue consuming frames until success/failure or transport failure, and test with deliberate chatter. Evidence: !15927.

## Continuation

The corpus is not exhausted at this commit: `mr_15898.json` is present. The next run must again rebuild the exact reviewed set from all tracking before selecting candidates; subject to that reconstruction, !15898 is a candidate, but no contiguous-range assumption should be made.