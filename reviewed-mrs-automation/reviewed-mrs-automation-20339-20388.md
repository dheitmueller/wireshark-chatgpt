# Wireshark MR review automation ledger — !20388 through !20339

Corpus: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Direction: newest available previously-unreviewed MRs toward older MRs.

This run reviewed exactly **50** previously unreviewed merge requests. Selection was based on the union of individual entries in `reviewed-mrs.md`, the aggregate automation tracking, and the per-run files under `reviewed-mrs-automation/`; numeric spans were not treated as implicitly complete. The historical !17571-!17620 batch remains preserved and counted.

The named gaps in later ledgers do not create higher-numbered work for this run: !22966 and !21133/!21132 are absent from the pinned corpus; !22662 and !22208 were already individually reviewed. The previous run ended at !20389, making !20388 the highest existing unreviewed corpus MR.

## Exact MRs reviewed

`!20388 !20387 !20386 !20385 !20384 !20383 !20382 !20381 !20380 !20379 !20378 !20377 !20376 !20375 !20374 !20373 !20372 !20371 !20370 !20369 !20368 !20367 !20366 !20365 !20364 !20363 !20362 !20361 !20360 !20359 !20358 !20357 !20356 !20355 !20354 !20353 !20352 !20351 !20350 !20349 !20348 !20347 !20346 !20345 !20344 !20343 !20342 !20341 !20340 !20339`

Count: **50**.

## High-value findings

- **!20384 — deep, merged master, very high weight.** John Thacker moved postdissector first-pass field priming into libwireshark's common dissection path. The library itself knows whether the frame is visited and whether registered postdissectors want fields, so frontends no longer need to remember a separate priming call. Anders Broman approved/merged it. Promoted to `dissector-pipeline-conventions.md`: pipeline invariants derivable by the owning layer should be enforced by that layer rather than delegated to every application caller.
- **!20378 — deep, merged master, very high weight.** John Thacker demonstrated that unloading plugins before LeakSanitizer reports turns plugin frames into `<unknown module>`. ASan/LSan builds therefore keep plugins resident through process exit so leak reports retain symbol/file/line information. Promoted to `build-conventions.md`: instrumentation builds may narrowly alter teardown when necessary to preserve actionable diagnostics.
- **!20361 and !20376 — architecture/corroboration, merged.** Broad conversions replace ambient `wmem_packet_scope()` with explicit `pinfo->pool` or passed allocators, including plumbing `packet_info *` through helper layers. These strongly corroborate the existing allocator-scope rule that packet-owned allocations should derive their lifetime from explicit packet context rather than ambient scope state.
- **!20353 + !20354 — deep sequencing pair, merged.** Gerald Combs changed epan field-idleness tracking to include the originating data-source TVB; John Thacker then noticed that the new comparison was occurring before `fi->ds_tvb` had been initialized and immediately supplied !20354 to move initialization ahead of the check. This is strong corroboration that validation must run only after every state component it compares has been established; the later corrective MR carries more implementation weight than the transient ordering in !20353.
- **!20345 — discussion-focused, merged.** The Modbus/TCP change deliberately preserves enough classification checks to avoid labeling arbitrary port-502 traffic as Modbus while moving semantic inconsistencies into normal dissection and Expert Info so broken implementations remain inspectable. Michael Mann explicitly objected to simply removing discriminators; the accepted version balances protocol recognition against malformed-protocol diagnostics and includes a malformed sample capture. This corroborates existing heuristic/classification and malformed-input guidance rather than creating a duplicate rule.
- **!20339 — merged cleanup.** COPS moves from text-formatting helpers toward registered native protocol-tree fields and away from deprecated APIs, reinforcing searchable/filterable semantic fields rather than presentation-only dissector output.

## Corroborating clusters and scanned work

- **MATE ownership/lifecycle:** !20388, !20383, !20382, !20381 and draft !20380 examine dead runtime tables, parser-token ownership, hash-table cleanup, and broader MATE memory lifetime. The merged focused cleanups carry more weight than the broader draft proposal; existing configuration/runtime-state and ownership rules already cover the durable themes.
- **Explicit packet allocator context:** !20376, !20361 and !20351 continue the project-wide move from `wmem_packet_scope()` toward `pinfo->pool`/explicit allocator parameters. No duplicate allocator rule was added.
- **Qt/runtime lifecycle:** !20370 fixes column reconstruction when Lua plugins are reloaded; !20359 adapts filtering code to Qt 6.9/6.10 API evolution; !20365/!20366/!20368 form a related field-value-representation correction series. These were inspected as UI/API lifecycle evidence but did not justify a new cross-cutting convention.
- **Protocol correctness/extensions:** !20387/!20386 (GSM SIM), !20369 (ETSI CAT), !20367 (Zigbee ZCL), !20362 (GSM SIM terminal profile), !20360/!20358/!20371 (UDS), !20357 (GSM RP), !20356 (O-RAN), !20350 (SMB2), !20346 (SUSPEND UICC), !20344 (JSON-3GPP), !20343 (TETRA), !20342 (RTCP padding), and !20341 (RPKI/Coverity) were reviewed for generalized lessons; most are protocol-specific or corroborate existing boundary/type/static-analysis rules.
- **Static-analysis/checker cleanup:** !20355 and !20347 improve or respond to `check_typed_item_calls.py`; !20341 follows a Coverity finding. These support the notebook's existing view that project checkers expose semantic field/API defects, not just style issues.
- **Linux cooked GRE handling:** !20340/!20348/!20349 are repeated variants of the SLL IP6GRE/IPGRE interpretation fix. They were treated as one implementation lineage rather than three independent architectural signals.
- **Automatic maintenance:** !20372/!20373/!20374 are automatic generated-data updates and were inspected but assigned low convention weight.

## Weighting / non-promotions

Merged master changes with explicit rationale, concrete before/after evidence, or substantive maintainer review received the most weight. John Thacker's authored !20384 and !20378 receive very high confidence; !20353/!20354 is particularly useful because the follow-up directly identifies and fixes an initialization-order mistake in the preceding merged change. Open **!20364** and draft/broader **!20380** were deliberately down-weighted and were not treated as accepted architecture.

No Guy Harris discussion in this 50-MR slice supplied a stronger new durable convention than the merged maintainer evidence above; Guy-authored/reviewed material elsewhere in the notebook remains weighted according to the established policy.

## Notebook changes from this run

- `dissector-pipeline-conventions.md`: added the rule that mandatory dissection preparation derivable from libwireshark state should be internalized at the common library entry point, based on merged !20384.
- `build-conventions.md`: added sanitizer-build guidance to preserve dynamic-module residency when unloading would destroy symbolized ASan/LSan diagnostics, based on merged !20378.
- Existing allocator-scope, parser/classification, ownership, static-analysis, and field-semantic rules were reused for the corroborating MRs instead of adding duplicates.
