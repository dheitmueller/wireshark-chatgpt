# Wireshark MR review automation: !16609-!16658

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Direction: descending from newest toward older MRs. Before selection, the existing per-run ledgers in `reviewed-mrs-automation/` and `reviewed-mrs.md` were consulted; the historical !17571-!17620 batch remains counted. This run selected the fifty highest-numbered corpus MRs not already represented in that tracking.

## Exact reviewed set

!16658, !16657, !16656, !16655, !16654, !16653, !16652, !16651, !16650, !16649, !16648, !16647, !16646, !16645, !16644, !16643, !16642, !16641, !16640, !16639, !16638, !16637, !16636, !16635, !16634, !16633, !16632, !16631, !16630, !16629, !16628, !16627, !16626, !16625, !16624, !16623, !16622, !16621, !16620, !16619, !16618, !16617, !16616, !16615, !16614, !16613, !16612, !16611, !16610, !16609.

Count: **50**.

## Review notes

Merged master changes were weighted most heavily; release backports, abandoned work, and low-information changes were treated mainly as corroboration.

- **!16658 — MDB Bill Validator dissector (merged master).** Long-lived first-contribution MR with extensive maintainer review. The final accepted change extends the existing MDB dissector rather than creating an unrelated parallel protocol implementation. Review included naming/field correctness and iterative cleanup. Useful mainly as corroboration for careful field semantics and maintainer-guided convergence; no new project-wide rule was extracted.
- **!16650 — documentation `.gitignore` update (merged master, Gerald Combs).** Keeps generated documentation paths synchronized after the doc-tree consolidation. Straightforward repository-maintenance evidence; no new convention.
- **!16640 — NTLMSSP hash-key lifetime fix (merged release-4.2 backport of master fix, John Thacker).** A stack-backed key must not be retained in a hash table after its lifetime ends. The accepted fix allocates a permanent key copy and gives the hash table key-destruction ownership. This strongly corroborates existing ownership/lifetime guidance, but as a backport is weighted below its originating master change.
- **!16630 — BPv7 timestamp initialization (merged master, John Thacker).** Zero-initializes a struct before a helper populates it. Corroborates existing defensive-initialization/static-analysis guidance without establishing a new rule.
- **!16620 — ASN.1 generator warning cleanup / UTF-8 requirement (merged master, Gerald Combs).** Cleans stale ASN.1 rename/attribute directives, requires ASN.1 source text to decode as UTF-8, and makes the CI regeneration check less noisy and broader. This reinforces treating generated-dissector regeneration as a first-class CI invariant and keeping generator inputs warning-clean. Existing notebook ASN.1/generated-code guidance already covers the core workflow, so no duplicate rule was added.
- **!16610 — CIGI 4 dissector-table conversion (merged master, John Thacker).** Replaces closed packet-ID dispatch with a dissector table, enabling external/Lua registration for locally defined packet IDs; also reuses common fields shared with CIGI 3 and fixes incorrect packet-length offsets. Strong accepted example of using dissector tables at extensibility points and reusing semantically identical hf fields rather than cloning version-specific fields. This is consistent with existing dissector-table/helper-reuse guidance.
- **!16609 — documentation target cleanup (merged master, Gerald Combs).** Moves `EXCLUDE_FROM_ALL` semantics from the whole documentation directory to the expensive guide targets themselves, preserving useful doc targets while keeping guides out of the default build. Build-system-specific and not a new general convention.

The remaining MRs in the exact set were scanned for state, target branch, purpose, diff shape, and substantive discussion. None provided a stronger new durable coding/architecture/testing/submission rule than the already-recorded notebook guidance. In particular, backports and mechanical/maintenance changes were not allowed to outweigh merged master exemplars.

## Notebook outcome

No convention file was changed in this run. The durable findings reinforce existing guidance on ownership/lifetime, defensive initialization, generated ASN.1 workflows, helper/field reuse, and dissector-table extensibility rather than adding a distinct rule.

## Continuation

Rebuild the reviewed set from all tracking before the next run. Subject to that check and the current corpus snapshot, the next descending candidate is **!16608**.
