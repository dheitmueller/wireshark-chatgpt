# Automated Wireshark MR review ledger: !22242-!22291

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`
Review direction: descending MR number, newest to older.
Selection method: rebuilt the already-reviewed MR set from all available `reviewed-mrs-automation/` ledgers plus `reviewed-mrs.md`; retained the historical !17571-!17620 batch. The fifty highest-numbered corpus MRs not in that set were !22291 through !22242 inclusive.

Exactly 50 MRs were reviewed in this run:

- !22291 — merged; spelling cleanup; no new durable convention.
- !22290 — merged; NetPerfMeter payload-length boundary fix; corroborates validating offsets/lengths against actual wire layout.
- !22289 — merged; const-ifies immutable dissector tables; useful implementation cleanup, no new notebook rule.
- !22288 — merged release backport; asn2wrs BER UTCTime `VAL_PTR`; corroborates generated-code/directive semantics.
- !22287 — merged release backport; L2CAP incomplete-reassembly fix; corroborates !22283.
- !22286 — merged release backport; L2CAP incomplete-reassembly fix; corroborates !22283.
- !22285 — merged release backport; SNMP BulkPDU request-id field abbreviation compatibility; corroborates filter-field stability.
- !22284 — merged master; LTP client-service dispatch moved through a dissector table with recursion protection; useful subdissector architecture evidence.
- !22283 — merged master, John Thacker; do not publish an L2CAP reassembly unless accumulated bytes equal the expected total; promoted to `reassembly-conventions.md`.
- !22282 — merged master, John Thacker; restores a common SNMP request-id filter abbreviation and was explicitly selected for 4.6 backport; corroborates field/filter compatibility.
- !22281 — merged master; CBOR embedded-byte-string handling uses an extensible heuristic dissector list; corroborates common heuristic registration mechanisms.
- !22280 — merged; further `value_string_ext` conversion; performance/lookup cleanup, no new rule.
- !22279 — merged master, Michael Mann; injects application flavor into capture option handling while keeping dumpcap independent; corroborates application-layer dependency-direction rules.
- !22278 — merged release backport; ACDR filtering crash fix; no new durable rule beyond the master fix.
- !22277 — merged release backport; ACDR filtering crash fix; no new durable rule beyond the master fix.
- !22276 — merged; asn2wrs BER UTCTime `VAL_PTR`; master/release counterpart of !22288, no duplicate rule.
- !22275 — closed draft, explicitly illustrative and never intended to merge; down-weighted. Its stated goal of detecting dissector/UI coupling only corroborates accepted !22245/!22247.
- !22274 — merged release backport; L2CAP first-fragment reassembly fix; corroborates !22271.
- !22273 — merged release backport; L2CAP first-fragment reassembly fix; corroborates !22271.
- !22272 — merged master; ACDR filtering crash fix; implementation-specific correctness fix, no new generalized convention promoted.
- !22271 — merged master, John Thacker; copy only bytes actually present in an L2CAP fragment, not advertised final SDU length; promoted to `reassembly-conventions.md`.
- !22270 — merged; PPPoE cleanup after removing an `if (tree)` guard; corroborates !22264 and existing tree-independence guidance.
- !22269 — merged; Mongo uses common TVBuff Snappy decompression helper instead of manual buffer management; corroborates common TVBuff/helper use.
- !22268 — merged; plugins use `epan_get_environment_prefix()`; corroborates centralized environment/application policy.
- !22267 — merged; TPNCP data file lookup supports plugin directory; no new generalized rule promoted.
- !22266 — merged release backport; LTP SDA offset fix; no new generalized rule beyond master fix.
- !22265 — merged release backport; LTP SDA offset fix; no new generalized rule beyond master fix.
- !22264 — merged master, John Thacker; semantic `credit_offset` and subdissector TVB construction must not depend on `tree != NULL`; strong corroboration of existing tree-independence guidance.
- !22263 — merged master, Michael Mann; initializes postdissector registry in `packet_init()` rather than lazily in registration; promoted to `initialization-lifecycle-conventions.md`.
- !22262 — merged master, Michael Mann; initializes stat-tap/stat-tree registries from `epan_init()` rather than registration-time lazy allocation; promoted to `initialization-lifecycle-conventions.md`.
- !22261 — merged; Bluetooth L2CAP FCS validation; protocol-specific checksum correctness, no new notebook rule.
- !22260 — merged master after extended review; broad SMB1/SMB2 correctness fixes. Review history included CI/rebase churn; accepted protocol fixes did not justify a new cross-cutting convention.
- !22259 — merged; replaces manual captured-length handling with TVBuff reported-length semantics; corroborates existing TVBuff length-domain guidance.
- !22258 — merged; more `value_string_ext` conversion; no new durable rule.
- !22257 — merged; PostgreSQL CopyBoth response dissection reuses CopyIn/CopyOut wire format; protocol-specific extension.
- !22256 — merged; USB HID GET_DESCRIPTOR display support; protocol-specific field enhancement.
- !22255 — merged; Clang Analyzer dead-store cleanup; static-analysis maintenance, no new cross-cutting rule.
- !22254 — merged; fixes Export Objects reset leak by freeing pointed-to entries; corroborates ownership/lifecycle discipline.
- !22253 — merged master; LTP SDA offset fix; protocol-specific cursor correction.
- !22252 — merged; COSE_Key fix plus current IANA algorithm registry additions; protocol/registry update, no new generalized rule.
- !22251 — merged; `value_string_ext` selection/reordering; lookup performance cleanup.
- !22250 — merged; temporarily allows macOS CI job failure because hosted runners could not download prerequisites; operational exception, not a durable coding convention.
- !22249 — merged master, Michael Mann; explicitly initializes conversation, Export Objects, Follow Stream, RTD, and SRT registries; promoted to `initialization-lifecycle-conventions.md`.
- !22248 — merged master, Michael Mann; failed tap registration is surfaced to the user rather than causing an unexplained immediate process exit; useful UI diagnostics behavior, no separate rule promoted.
- !22247 — merged master, Michael Mann; keeps WSLua from depending on an exported DCE/RPC-specific helper by handling adaptation locally; promoted with !22245 to application-layer boundary guidance.
- !22246 — merged; uses the correct `proto_tree_add_guid_format_value` API as identified by precommit checks; corroborates API/field presentation correctness.
- !22245 — merged master, Michael Mann; moves DCE/RPC Decode-As specialization behind the generic `decode_as_t` abstraction so UI does not explicitly link to the dissector; promoted to `application-layer-boundary-conventions.md`.
- !22244 — merged; HTTP/2 IMSI state preservation fix; protocol-specific state correctness.
- !22243 — merged; ExportObjectDialog no longer application-modal, consistent with other statistics tap dialogs; UI behavior cleanup.
- !22242 — merged master, John Thacker; prevents negative FCoE frame length before CRC subtraction; corroborates existing arithmetic/domain-validation guidance.

## Durable notebook changes from this batch

1. Added `initialization-lifecycle-conventions.md`: initialize subsystem registry/container state at explicit owning lifecycle entry points rather than lazily during first registration. Primary evidence: merged master !22249, !22262, !22263 (Michael Mann).
2. Added `reassembly-conventions.md`: copy/advance by bytes actually present in fragments and publish a completed reassembly only when accumulated length proves the advertised aggregate is complete. Primary evidence: merged master !22271 and !22283 (John Thacker), with release backports !22273/!22274 and !22286/!22287.
3. Extended `application-layer-boundary-conventions.md`: generic UI/extension layers should not explicitly link to individual dissector implementations; use generic callbacks/registries or localized adapters. Primary evidence: merged master !22245 and !22247 (Michael Mann). Closed illustrative !22275 was deliberately down-weighted.

Existing rules were not duplicated for !22264/!22270 (`tree` must not control semantic dissection), !22259 (TVBuff captured/reported lengths), !22242 (safe arithmetic), or !22279 (application-policy dependency direction).
