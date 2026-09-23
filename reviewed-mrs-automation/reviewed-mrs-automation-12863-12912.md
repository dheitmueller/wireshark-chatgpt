# Automated Wireshark MR review: !12863-!12912

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Review direction: descending from the newest available previously-unreviewed MR.

Selection was made from the explicit already-reviewed tracking in `reviewed-mrs.md`, the supplemental automation tracker, and the available per-run ledgers under `reviewed-mrs-automation/`. The historical !17571-!17620 batch remains counted. The previous !12912 lookup was only a frontier check and was not counted as reviewed. No reviewed-tracking match was found for any member of this candidate set, so this run reviews exactly the following 50 MRs:

`!12912, !12911, !12910, !12909, !12908, !12907, !12906, !12905, !12904, !12903, !12902, !12901, !12900, !12899, !12898, !12897, !12896, !12895, !12894, !12893, !12892, !12891, !12890, !12889, !12888, !12887, !12886, !12885, !12884, !12883, !12882, !12881, !12880, !12879, !12878, !12877, !12876, !12875, !12874, !12873, !12872, !12871, !12870, !12869, !12868, !12867, !12866, !12865, !12864, !12863`

All 50 selected corpus records are merged. Merged master work is weighted above stable-branch duplicates and routine generated/packaging changes.

## Durable findings

- **!12881 — deep, merged master, very high weight.** John Thacker documents the actual UAT lifecycle: `update_cb` validates the copy placed into `user_data`, but saving can later reconstruct `user_data` from already-validated `raw_data` without rerunning `update_cb`. Display-filter macros therefore lost parsed/derived state after save. The accepted fix makes the update path validation-only and rebuilds parsed macro state from `post_update_cb`, which is the callback guaranteed after table data changes. This sharpens the existing UAT copy/save convention and is recorded in `uat-callback-conventions.md`.

- **!12866 — deep, merged master, very high weight.** John Thacker fixes the first decryptable ZigBee APS packet by publishing a newly learned long address into the network-layer mapping as soon as the security header reveals it. Previously the state became useful only to subsequent packets because the higher APS layer in the same packet had already missed the mapping. This establishes a same-packet state-publication rule and is recorded in `protocol-context-state-conventions.md`.

- **!12867 — deep/corroborating, merged master, very high weight.** John Thacker fixes two illegal-memory/lifetime errors in RF4CE: bounded string comparison must stop at the NUL terminator when the source may be shorter than the fixed compare width, and stack-produced column text must use a copying API (`col_add_str`) rather than the borrowed-pointer `col_set_str` API. The column lifetime lesson is already captured strongly in `column-string-conventions.md` / `allocator-scope-conventions.md`, so no duplicate notebook rule was added.

- **!12909 — substantive, merged master.** Display-filter field-reference syntax is extended from `${field}` to also permit `$field`, with parser, documentation, release-note, and test changes kept together. This is a compatibility-preserving grammar extension: old syntax remains accepted while the new spelling maps to the same reference AST node. It corroborates the existing display-filter compatibility/testing guidance without requiring a new rule.

- **!12883 — substantive, merged master.** Removes undefined radiotap S1G NDP fields instead of continuing to expose a representation whose implementation/specification status was not sound. The accepted patch also folds in a small typo/indent cleanup. This reinforces the existing authoritative-source rule that registered protocol fields should correspond to a defensible specification model; no duplicate rule added.

- **!12907/!12905/!12904 and related BLF fixes — merged.** The cluster distinguishes captured payload length, declared payload semantics, and fixed-size CAN object layout, and corrects timestamp, metadata, leak, and warning issues in adjacent MRs. Useful wiretap robustness evidence, but the durable length/bounds and representation rules are already represented elsewhere in the notebook.

- **!12908 — substantive, merged master.** Adds display-filter IP-address functions through the plugin mechanism rather than hard-wiring specialized operations into the core expression syntax. This is a useful extensibility exemplar but did not add a stronger general rule than the notebook's existing plugin/application-boundary guidance.

- **!12891/!12894 — merged master/backport pair.** FT_BYTES textual rendering is bounded by the available label/column space instead of assuming one fixed display budget. Good presentation/API-boundary evidence, but too narrow to promote independently.

- **!12870/!12871/!12872 — merged backport family of the ZigBee first-packet decryption fix.** These corroborate !12866 across maintained branches without adding a separate convention.

Routine automatic data/translation updates, packaging/symbol maintenance, Python 3.12 warning cleanups, straightforward backports, spelling fixes, and small UI sizing changes were counted but intentionally not promoted into durable notebook guidance.

## Notebook updates

- `uat-callback-conventions.md`: added the distinction between validation-oriented `update_cb` and `post_update_cb` for rebuilding derived state after every accepted UAT content change, based on !12881.
- `protocol-context-state-conventions.md`: added the rule to publish newly discovered mapping/state before later dissector layers in the same packet consume it, based on !12866.

## Frontier

`!12862` exists in the corpus (`GitLab CI: Add Windows package release info`) and was inspected only as the next descending frontier candidate. It is **not** counted as reviewed by this ledger.
