# Wireshark MR Review Batch — !20289 through !20338

Corpus revision reviewed: `dheitmueller/wireshark-corpus-mrs@9e52bc78659a888d4eb624984ee1a886a40d959f`

## Selection and de-duplication

Before selecting this batch, the already-reviewed set was rebuilt from all available tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md`, the supplemental automation tracking, and the per-run files under `reviewed-mrs-automation/`. Numeric ranges were not assumed to be complete merely because individual MRs in them appeared in a ledger. The historical !17571–!17620 batch remains explicitly preserved and counted as reviewed.

The immediately preceding backward-review batch was !20339–!20388. All corpus objects !20338 through !20289 exist at the pinned corpus revision and were not found in the already-reviewed set. They therefore form the next fifty highest-numbered previously unreviewed MRs.

## Exact reviewed set

!20338, !20337, !20336, !20335, !20334, !20333, !20332, !20331, !20330, !20329,
!20328, !20327, !20326, !20325, !20324, !20323, !20322, !20321, !20320, !20319,
!20318, !20317, !20316, !20315, !20314, !20313, !20312, !20311, !20310, !20309,
!20308, !20307, !20306, !20305, !20304, !20303, !20302, !20301, !20300, !20299,
!20298, !20297, !20296, !20295, !20294, !20293, !20292, !20291, !20290, !20289.

Count: **50**.

## Review weighting and durable findings

- **!20317 — Deep, merged master, very high weight.** John Thacker documented the `col_set_str()` lifetime contract: it retains the supplied pointer rather than copying it, so `wmem_packet_scope()` storage is freed too early while `pinfo->pool` has the required lifetime. Martin Mathieson pointed to `./tools/check_col_apis.py --verbose` as a screening tool, and John noted that violations can be non-local across functions. Promoted to `allocator-scope-conventions.md`.
- **!20316, !20311, !20319, !20332 — Deep/corroborating, merged.** These independently move dissector allocations from ambient `wmem_packet_scope()` to explicit `pinfo->pool`, including concrete use-after-free/crash cases and a NULL-`pinfo` presentation path. They strengthen the !20317 lifetime rule without requiring duplicate notebook sections.
- **!20320 — Deep, merged master, highest review authority in this batch.** A floating-point custom-column overflow fix propagates the actual destination capacity through formatting helpers. Guy Harris explicitly reviewed the formatting strategy; the accepted implementation also audits the sibling IEEE 11073 formatter. Promoted to `string-api-conventions.md`.
- **!20337 — Deep, merged master.** John Thacker identified that `g_list_find()` was comparing string pointers rather than string contents, so an assertion intended to enforce unique menu names was ineffective. He also questioned whether the old GTK-era invariant itself was still necessary. Promoted to `comparison-semantics-conventions.md`.
- **!20330 — Deep/corroborating, merged master; John Thacker-authored and Gerald Combs-approved.** Replaces wildcard Qt signal disconnection with the exact signal/slot relationship owned by the application. This independently confirms the existing `qt-signal-connection-conventions.md` rule; no duplicate rule was added.
- **!20302 — Deep, merged master.** Moves DIAMETER/GTP/GTPv2/PFCP session tracking before generated association fields are added to the tree, so the first request that creates the relationship can display it immediately. Promoted to `dissector-pipeline-conventions.md`.
- **!20328 — Deep, merged master.** New ILNP dissector received substantive maintainer review. Michael Mann requested a squashed submission with a commit message matching the MR description and a sample capture for regression testing; Stig Bjørlykke rejected an unnecessary compile-time feature guard absent an optional third-party dependency. These corroborate existing submission/testing and build-configuration conventions rather than creating new rules.
- **!20308 with release-4.4 backport !20334 — Deep/merged.** The Show Packet Bytes dialog takes an owned copy of selected tvbuff bytes so the dialog no longer retains capture-file/selected-packet storage beyond its lifetime. Useful ownership corroboration; no separate rule was needed.
- **!20333 — Discussion-light but technically useful, merged master, John Thacker-authored.** Converts an SMB persistent-state allocation to `wmem_new0()` after a newly added field was left uninitialized, reducing the risk that future zero/false/NULL-default fields require scattered manual initialization. Retained in this ledger as an implementation exemplar but not promoted as a standalone notebook rule.
- **!20335 — Discussion-focused, closed/unmerged; deliberately down-weighted.** The proposed plugin-menu mnemonic behavior raised toolkit-independence concerns from John Thacker because Qt ampersand semantics would leak into Lua/plugin source. Because the MR was ultimately closed, it is context rather than accepted precedent.
- **!20289 — Scanned, merged release-4.4 backport.** Besides its Gryphon offset correction, it avoids formatting arbitrary packet bytes with `%c` because fuzzed byte values are not guaranteed to form valid UTF-8. Useful corroborating output-safety evidence, but as a backport it was weighted below merged master examples already represented in the notebook.

The remaining MRs in the exact set were inspected for state, purpose, diff scope, and available discussion. They were predominantly localized dissector correctness fixes, backports, generated-data updates, documentation/build cleanups, logging conversions already covered by existing `ws_log` guidance, or changes without additional durable reviewer feedback. They count as reviewed but did not justify new notebook rules.

## Notebook changes from this run

- Extended `allocator-scope-conventions.md` with the borrowed-pointer lifetime contract for `col_set_str()` and the `check_col_apis.py` screening practice.
- Extended `string-api-conventions.md` with caller-provided destination-capacity requirements for formatting helpers, based on !20320 and Guy Harris's review.
- Added `comparison-semantics-conventions.md` for semantic-value versus pointer-identity comparisons, based on !20337.
- Extended `dissector-pipeline-conventions.md` with state-before-presentation ordering for current-packet-derived state, based on !20302.
- Existing Qt wildcard-disconnect guidance was left in its existing notebook file and treated as corroborated by !20330 rather than duplicated.
