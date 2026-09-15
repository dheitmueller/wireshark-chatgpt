# Wireshark MR review automation ledger — !19240–!19289

Corpus commit reviewed: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

This run reviewed exactly 50 previously unreviewed merge requests. Selection was rebuilt from `reviewed-mrs.md` and the per-run ledgers in `reviewed-mrs-automation/`, including the preserved historical !17571–!17620 batch. Numeric ranges were not treated as reviewed merely from their filenames. Existing tracking covers the newer frontier; after the prior !19290–!19339 run and subsequent backfill/newer runs, these are the fifty highest-numbered remaining unreviewed MRs.

## Exact MRs reviewed

`!19289 !19288 !19287 !19286 !19285 !19284 !19283 !19282 !19281 !19280 !19279 !19278 !19277 !19276 !19275 !19274 !19273 !19272 !19271 !19270 !19269 !19268 !19267 !19266 !19265 !19264 !19263 !19262 !19261 !19260 !19259 !19258 !19257 !19256 !19255 !19254 !19253 !19252 !19251 !19250 !19249 !19248 !19247 !19246 !19245 !19244 !19243 !19242 !19241 !19240`

Count: **50**.

## Review notes

The batch was reviewed from the corpus records, weighting merged results above closed/draft/superseded submissions and giving maintainer-authored/reviewed material additional weight.

- **!19260 — Deep, merged, John Thacker:** documents that `proto_free_deregistered_fields()` is unsafe from dissectors, including preference callbacks, because references to deregistered fields may remain live. Companion allocations such as dynamic `hf_id` arrays should be registered with `proto_add_deregistered_data()` and reclaimed at the framework-owned safe lifecycle point. Promoted to `field-deregistration-lifecycle-conventions.md`.
- **!19247 — Deep, merged:** broad endianness cleanup. Historical boolean interpretation made `ENC_NA` accidentally behave like big endian in some contexts; accepted code uses the actual encoding domain. Guy Harris's review emphasizes testing the intended enum/bit semantics directly and keeping branch meaning clear. Strong corroboration of existing encoding-domain guidance; no duplicate rule added.
- **!19288 — Deep, merged, Gerald Combs with Guy Harris discussion:** extends pcapng/libscap event-block option handling so event comments round-trip across Stratoshark/libscap-compatible producers. Useful interoperability evidence, but no new convention beyond existing pcapng extension/option handling guidance.
- **!19289, !19283, !19282 — merged Elastic/JSON output fixes:** keep generated Elasticsearch mappings/output aligned with the external schema, including mandatory `index_patterns`, correct field-type mappings, and removal of deprecated `_type`. Corroborates existing machine-output/schema-contract guidance.
- **!19286/!19285 — merged display-filter crash fix:** fixes BASE_CUSTOM value-string formatting by returning the populated buffer correctly. Correctness fix; no new general rule.
- **!19287, !19281, !19280, !19279, !19278, !19276, !19275, !19274, !19273, !19268, !19264, !19258, !19255, !19250, !19242, !19240 — merged protocol/ftype correctness and feature work:** protocol-specific additions or corrections; useful accepted-code evidence but no distinct durable repository convention beyond existing field semantics, standards fidelity, and regression guidance.
- **!19277 and !19270 — closed/draft:** down-weighted relative to merged successors/accepted work and not used as architecture exemplars.
- **!19269 — merged tooling help correction; !19272 — merged analyzer dead-store cleanup; !19265 — merged Coverity-driven OER fix; !19259/!19256 — merged field-size corrections; !19254/!19261 — linkage visibility cleanups:** corroborate existing compiler/static-analysis/API-surface guidance.
- **!19267/!19263/!19262 — s7comm heuristic/reassembly sequence:** reviewed as a related sequence rather than treating each revision as independent evidence; reinforces existing heuristic handoff/reassembly guidance.
- **!19266 — merged zlib/zlib-ng compatibility-header refactor:** centralizes repeated compatibility boilerplate; good maintainability evidence but no new architectural rule.
- **!19253/!19249 — tshark null-EDT lifecycle fix sequence:** avoids cleanup/init on a null `epan_dissect_t`; corroborates existing lifecycle/precondition guidance.
- **!19252 — merged SMB2 pipe subdissector work; !19251 — merged ELF-over-HTTP heuristic:** extension-point/heuristic examples already covered by existing notebook conventions.
- **!19248 — merged IEEE-11073 negative-float filter/compare restoration:** reinforces that ftype parsing, comparison and display semantics must agree, already represented by ftype/filter conventions.
- **!19246 — Lua UAT preference support:** accepted scripting/API extension; no additional durable rule extracted in this run.
- **!19245/!19244/!19243 — automatic updates:** no durable engineering-review lesson.
- **!19241 — merged display-filter diagnostic correction:** diagnostic text must match the numeric representation actually emitted; narrow correctness cleanup.

## Durable notebook change

Added `field-deregistration-lifecycle-conventions.md` from merged John Thacker !19260. The key distinction is that field deregistration is not immediate destruction: dissectors and preference callbacks must leave actual reclamation to epan's safe lifecycle point and attach companion allocations using the supported deferred-data mechanism.

## Selection continuity

After this run, the descending frontier is below **!19240**, subject to exact reconstruction of any older individual holes. Future runs must continue selecting from individual ledger entries rather than assuming numeric range completeness.
