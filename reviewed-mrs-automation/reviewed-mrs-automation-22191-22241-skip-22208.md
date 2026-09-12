# Automated Wireshark MR review ledger: !22191-!22241 (skip !22208)

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`
Review direction: descending MR number, newest to older.
Selection method: rebuilt the already-reviewed MR set from all available `reviewed-mrs-automation/` ledgers plus `reviewed-mrs.md`, preserving and counting the historical !17571-!17620 batch. MR !22208 was already recorded in `reviewed-mrs.md`, so it was excluded individually rather than assuming a numeric range. The fifty highest-numbered corpus MRs not already reviewed were therefore !22241 through !22209, followed by !22207 through !22191.

Exactly 50 MRs were reviewed in this run:

- !22241 — merged master; BT-DHT version field gets semantic/searchable fields while retaining the prior syntax-oriented node hidden for compatibility. Substantive Alexis La Goutte review; useful field-evolution evidence but no new cross-cutting rule promoted.
- !22240 — merged master, authored and merged by John Thacker; suppresses libxml2 diagnostics while a 3GPP Nettrace opener is still only probing a candidate file. Promoted to `wiretap-file-detection-conventions.md`.
- !22239 — merged master, authored by John Thacker; broad TVBuff subset cleanup corrects reported/captured-length confusion and truncation behavior. Strong corroboration of existing TVBuff length-domain guidance.
- !22238 — merged release/backport counterpart; ISOBUS UTF-16 BOM endianness correction. No additional general rule.
- !22237 — merged release/backport counterpart; same ISOBUS UTF-16 endianness correction. No additional general rule.
- !22236 — merged release/backport counterpart; Ethernet CFM BNM PDU dissection correction. No additional cross-cutting lesson.
- !22235 — merged release/backport counterpart; Ethernet CFM BNM PDU dissection correction. No additional cross-cutting lesson.
- !22234 — merged; tunes `value_string_ext` instances to use indexed lookup where appropriate. Performance/table-maintenance cleanup, no new notebook rule.
- !22233 — merged automatic data/translation update; no substantive review lesson.
- !22232 — merged automatic data/translation update; no substantive review lesson.
- !22231 — merged automatic data/translation update; no substantive review lesson.
- !22230 — merged master; NFLOG enhancement including Ethernet-header and payload nesting changes. Protocol-specific architecture work; no new generalized convention promoted.
- !22229 — merged master; Ethernet CFM BNM PDU dissection fix. Protocol-specific correctness.
- !22228 — merged master; ISOBUS UTF-16 BOM endianness fix. Protocol-specific correctness.
- !22227 — closed/unmerged SMB FID-subtree proposal; deliberately down-weighted.
- !22226 — merged master new NATS dissector after substantive maintainer review. Michael Mann challenged unnecessary header exposure unless data really crosses the dissector boundary; Alexis La Goutte requested a pcap and fuzzing; Martin Mathieson supplied the project fuzz/Valgrind workflow; author reported 17,500 fuzz rounds and planned generated non-sensitive captures because production captures were sensitive. Strong corroboration of existing sample-capture/fuzzing guidance.
- !22225 — closed/superseded first NATS submission; down-weighted in favor of merged !22226.
- !22224 — merged master, authored by John Thacker; further subset-TVBuff simplification and captured/reported-length corrections. Corroborates existing TVBuff guidance.
- !22223 — merged master, authored and merged by John Thacker; OSS-Fuzz field-width and C integer-promotion/signed-overflow fixes, plus correct reported length for truncation. Strong corroboration of existing type/arithmetic and TVBuff rules.
- !22222 — merged master; DCE/RPC Decode As crash fix handles absence of an active DCE/RPC packet and aligns lifetimes of GUID keys and decode data. Useful lifecycle/UI robustness evidence; no separate general rule promoted.
- !22221 — merged master new RTTrP dissector. Protocol-specific addition; no new cross-cutting lesson identified.
- !22220 — merged master, Michael Mann, reviewed/approved/merged by John Thacker; moves tap-listener registration into `epan_init()` because listener-owned UATs must exist before UAT data loading. Promoted to `initialization-lifecycle-conventions.md`.
- !22219 — merged release backport; peektagged maximum-section-size update. No additional rule.
- !22218 — merged master; adds Ruff to commit-check CI. Corroborates existing project-CI/tooling guidance.
- !22217 — merged master; broad `tvb_new_subset_length_caplen` simplification. Corroborates existing TVBuff captured/reported-length semantics.
- !22216 — merged; checker verifies ascending `value_string` values under extra checks. Corroborates executable pre-submit validation, no separate rule.
- !22215 — merged master; peektagged maximum-section-size correction. Format-specific fix.
- !22214 — merged master; O-RAN preference for deployments whose UL C-Plane compression settings cannot be trusted. Protocol/interoperability-specific behavior.
- !22213 — merged; Windows WinSparkle dependency/layout update. Build/dependency maintenance only.
- !22212 — merged successor to already-reviewed !22208; LLRP Impinj Doppler parameter support with before/after evidence and an attached named pcap. Corroborates sample-capture expectations.
- !22211 — merged master, authored by John Thacker; subset-TVBuff simplification. Corroborates existing TVBuff guidance.
- !22210 — open snapshot with extensive Qt color-rule refresh discussion; not accepted in the corpus snapshot, so deliberately down-weighted and not used as durable architecture evidence.
- !22209 — closed/unmerged memory-lifetime proposal; deliberately down-weighted.
- !22207 — merged; removes known-broken checksum example code from extcap example rather than leaving misleading sample code. Maintenance cleanup, no new generalized rule.
- !22206 — merged master, authored and merged by John Thacker; replaces `-1`/manual subset lengths with `tvb_new_subset_remaining()` and remaining-length APIs, explicitly avoiding overflow-prone `reported_length - offset`. Strong corroboration of existing TVBuff/arithmetic guidance.
- !22205 — merged; accepts observed 3GPP Nettrace `rawMsg` elements lacking payload despite stricter expected schema. Interoperability-specific tolerance.
- !22204 — merged; OSS-Fuzz Bencode signed-overflow fix uses unsigned domains for never-negative lengths and preserves original length for expert items. Corroborates existing C type/arithmetic guidance.
- !22203 — merged; includes HTTP/3 in HTTP coloring rule. UI/default-data maintenance.
- !22202 — merged; sharkd reuses existing `SAMPLE_BYTES` macro instead of duplicating calculation. Corroborates helper reuse.
- !22201 — merged; Couchbase opaque field endianness corrected using protocol consistency, memcache implementation, and SDK-log comparison. Good external/cross-implementation validation example; no new notebook rule.
- !22200 — merged; avoids printing an empty `With:` version-information section. UI/output cleanup.
- !22199 — merged master, authored and merged by John Thacker; OSS-Fuzz SUA signed-overflow fix documents C integer promotions and widens operands before combining 16-bit values. Strong corroboration of existing C-type/arithmetic guidance.
- !22198 — merged; removes Qt workaround only once upstream Qt contains the actual fix. Dependency-version maintenance.
- !22197 — merged; fixes default files examined by `check_tfs.py`. Checker maintenance.
- !22196 — merged master, authored and merged by Michael Mann; removes epan's global application-flavor dependency by passing structured application data into `epan_init()`. Corroborates existing application-layer dependency-direction guidance and provides the architectural context for !22220.
- !22195 — merged; fixes a copy/paste error in an ANSI TCAP `value_string`. Local correctness cleanup.
- !22194 — merged; Ruff-driven Python cleanup. Tooling maintenance, no new rule.
- !22193 — merged; ASAM CMP parsing refactor splits data-message parsing and constrains payload parsing with limited TVBs. Corroborates bounded-sub-TVBuff parser design.
- !22192 — merged; transitional removal of packet-frame application-flavor queries. Corroborates !22196 and existing application-boundary guidance.
- !22191 — merged; LI5G memory leak fix. Local ownership correctness, no new generalized convention promoted.

MR !22208 was not reviewed again in this run. It remains counted as previously reviewed from `reviewed-mrs.md` (Alexis La Goutte requested a named topic branch rather than submitting from the fork's `master`; the work was resubmitted as !22212).

## Durable notebook changes from this batch

1. Extended `wiretap-file-detection-conventions.md`: speculative openers should suppress/capture subordinate-parser diagnostics until they establish format ownership; ordinary `WTAP_OPEN_NOT_MINE` paths should be quiet. Primary evidence: merged master !22240, authored and merged by John Thacker.
2. Extended `initialization-lifecycle-conventions.md`: registration phases that contribute UATs/preferences/schema must run before configuration loading that consumes those registrations. Primary evidence: merged master !22220, authored by Michael Mann and explicitly endorsed by John Thacker, with !22196 providing the application-data initialization context.

Existing notebook rules were deliberately not duplicated for the repeated TVBuff subset conversions (!22239, !22224, !22217, !22211, !22206), integer-promotion/overflow fixes (!22223, !22204, !22199), application-boundary work (!22196/!22192), or NATS sample-capture/fuzzing review (!22226).
