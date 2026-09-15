# Wireshark MR review automation: !26078-!26127

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Selection method: rebuilt the already-reviewed set from `reviewed-mrs.md` and all available files under `reviewed-mrs-automation/`, preserving and counting the historical !17571-!17620 batch. Selected the 50 highest-numbered corpus MRs not already present in that reviewed set. The immediately newer !26128-!26177 batch is already tracked, so this run continues backward with !26127 through !26078.

Exact reviewed MRs (50):

`!26127, !26126, !26125, !26124, !26123, !26122, !26121, !26120, !26119, !26118, !26117, !26116, !26115, !26114, !26113, !26112, !26111, !26110, !26109, !26108, !26107, !26106, !26105, !26104, !26103, !26102, !26101, !26100, !26099, !26098, !26097, !26096, !26095, !26094, !26093, !26092, !26091, !26090, !26089, !26088, !26087, !26086, !26085, !26084, !26083, !26082, !26081, !26080, !26079, !26078`

## Review notes

Merged MRs were weighted more heavily than drafts, abandoned submissions, and backports. Backports were treated primarily as corroboration of the corresponding master fix rather than independent architectural evidence.

- **!26124 — RTCP: Strengthen heuristic (merged, John Thacker):** after Reduced-Size RTCP support weakened the discriminator, the heuristic adds a cheap structural invariant: at least four captured bytes and a reported length sufficient for the first RTCP header's encoded word length. Importantly it checks `>=`, not exact equality, because compound RTCP and SRTCP legitimately contain additional bytes. Strong corroboration of the existing notebook rule that heuristic dissectors should use mandatory structural invariants while allowing protocol-defined extensions/encapsulation rather than incidental packet shape.
- **!26125 — SAP DIAG variable-length item parsing (merged):** hardens variable-length parsing discovered while enabling compressed payload support. Corroborates existing bounded-container/length-validation guidance.
- **!26126 — SAP DIAG/RFC compressed payload dissection (merged):** builds on shared wsutil SAP decompression support rather than duplicating decompression logic in the dissector. Corroborates helper reuse and layered decoding guidance.
- **!26112 / !26108 / !26104 — BT AVRCP oversized reassembly hardening:** master/backport family prevents overflow from excessively large aggregate PDUs. Corroborates checked aggregate/reassembly arithmetic already recorded from adjacent batches.
- **!26110 / !26111 — UMTS RRC heap-corruption fix backports:** security/correctness backports of the master fix; no distinct convention beyond existing malformed-input bounds guidance.
- **!26105 / !26107 — DCT3 Trace extra `<l2>` overflow hardening:** malformed XML-like trace structure must not overrun fixed assumptions about child elements. Corroborates capacity/bounds rules.
- **!26106 / !26109 — BT AVRCP Coverity NULL-return handling:** static-analysis-driven defensive fix; corroborates existing static-analysis and nullable-return-contract guidance.
- **!26099 — etwdump LDAP ETW out-of-bounds fix:** source data reported with an exact byte size must not be treated as though it necessarily has an additional terminator. Corroborates existing source-length versus string/terminator contract guidance.
- **!26096 — ZigBee ZCL Touchlink commissioning-map redissection cleanup:** file-scope allocation lifetime does not imply logical state remains valid across redissection. This same state-lifecycle lesson is already represented in the notebook and was independently seen again in the later !26144 family.
- **!26093 / !26095 / !26097 — extcap control-pipe refactoring:** moves control-pipe ownership/handling out of Qt and toward the extcap/capture layer so non-Qt consumers can use it. This is useful architecture evidence for placing shared behavior below a UI-specific layer, but the notebook already contains the broader boundary/ownership rule.
- **!26098 — MCTP control command payloads:** extends the dissector beyond completion-code parsing into command-specific payloads; no new general convention extracted.
- **!26089 — MLE auxiliary-security fields:** supports caller-supplied registration fields for shared IEEE 802.15.4 auxiliary-security parsing, reinforcing shared parser reuse while preserving protocol-specific field namespaces.
- **!26088 — c-ares callback signatures:** follows upstream API const-correctness changes; primarily dependency/API maintenance.
- **!26085 — draft HyperDHT dissector:** draft/unmerged material was weighted below merged work and not used as authoritative convention evidence.
- **!26087 / !26086 / !26091 and related IDN submissions:** iterative/superseded development was weighted below the accepted successor state; no durable new convention extracted.
- **!26081 — radiotap zero-length PSDU presentation:** protocol-specific semantic/presentation refinement; no new cross-cutting convention.
- **!26078 / !26079 / !26080 — NAS 5GS service-level-AA fix family:** master plus backports; no additional general lesson beyond protocol-spec-driven field parsing.
- **!26084 / !26090 — Daintree timestamp-fraction validation:** malformed numeric input validation; corroborates existing bounded numeric parsing guidance.
- Remaining entries were release preparation, dependency/build fixes, automatic data updates, warning cleanup, protocol-specific value/field corrections, documentation, or narrow maintenance changes. They were inspected but did not justify duplicating already-established notebook rules.

## Notebook impact

No separate convention file was changed in this run. The strongest durable findings independently corroborate rules already present in the notebook: strong-but-permissive heuristic recognition, bounded variable-length parsing, checked reassembly arithmetic, semantic state reset across redissection, nullable API-result checking, and placing shared functionality below UI-specific layers. Duplicating those rules would reduce notebook signal-to-noise. This ledger is the notebook update for the run and is authoritative for avoiding duplicate review of these 50 MRs.
