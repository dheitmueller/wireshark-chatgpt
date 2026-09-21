# Wireshark MR review automation ledger: !14262-!14213

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`
Notebook starting commit: `92831c9dbe290f99e6eb05e41665ed0f0f5c40df`
Reviewed count: **50**
Status mix: **50 merged**.

## Selection and duplicate avoidance

Before selecting this batch, the reviewed set was reconciled against the available tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md` and the per-run files under `reviewed-mrs-automation/`. The historical **!17571-!17620** batch remains explicitly preserved and counted. The immediately preceding exact ledger, `reviewed-mrs-automation-26578-26580-plus-14263-14309.md`, was produced from a full reviewed-set reconciliation and explicitly records that !14262-!14260 had merely been probed and were **not** counted as reviewed.

Neither the notebook nor corpus had advanced after that exact ledger when this run began: notebook `main` was `92831c9dbe290f99e6eb05e41665ed0f0f5c40df`, and the corpus remained `ddcaa22b51c68f594e425a23388c3a2086813054`. Exact reviewed-set subtraction therefore makes the fifty highest-numbered unreviewed MRs in this snapshot:

`!14262, !14261, !14260, !14259, !14258, !14257, !14256, !14255, !14254, !14253, !14252, !14251, !14250, !14249, !14248, !14247, !14246, !14245, !14244, !14243, !14242, !14241, !14240, !14239, !14238, !14237, !14236, !14235, !14234, !14233, !14232, !14231, !14230, !14229, !14228, !14227, !14226, !14225, !14224, !14223, !14222, !14221, !14220, !14219, !14218, !14217, !14216, !14215, !14214, !14213`.

No numeric range was inferred from a partial ledger; this contiguous range is the result of exact prior-set reconciliation plus an unchanged corpus/notebook starting state.

## Review notes

| MR | State | Review | Durable assessment |
| --- | --- | --- | --- |
| !14262 | merged, master | **Deep / promoted** | F1AP propagates a UE identity into PDCP-NR metadata. Review explicitly rejected the DU-scoped ID because one CU can control several DUs and duplicate IDs can occur; the accepted implementation uses the CU-scoped UE ID. Promoted to `protocol-identity-key-conventions.md`: choose keys whose uniqueness domain covers every downstream consumer, or use a composite identity. |
| !14261 | merged, master | **Discussion-focused / promoted** | Adds the Travelping Diameter dictionary. Martin Mathieson noticed that the new XML was missing from both Windows NSIS manifests by comparing with a peer dictionary; the author added it to Wireshark and Logray installer lists before Anders Broman approved. Promoted to `packaging-data-conventions.md`. |
| !14260 | merged | Scanned / corroborating | Extends checker coverage for non-contiguous masks and related cleanup. Reinforces the existing notebook preference for encoding recurring declarative invariants in repository checkers rather than relying on reviewer memory. |
| !14259 | merged | Discussion-focused / corroborating | GTPv2 malformed F-Container handling reports an expert error while preserving parsing of subsequent IEs. Pascal Quantin also asked why a new expert field was introduced when the existing `ei_gtpv2_ie_len_invalid` already expressed the condition; the accepted revision reuses the existing diagnostic. Reinforces localized parser failure and reuse of semantically correct expert fields. |
| !14258 | merged | Scanned | Const-ifies enum/introspection data so read-only data can live in read-only storage. Useful cleanup; no separate convention needed. |
| !14257 | merged, master | **Deep / promoted; Guy Harris review** | Const-ifies generated registration tables and the exported tap-registration pointer contract. Guy Harris directly addressed the API/ABI question: adding read-only `const` to an argument/pointee is not inherently a binary ABI break, while still noting Wireshark's limited cross-major compatibility guarantees. Promoted to `abi-compatibility-conventions.md`. |
| !14256 | merged | Scanned / corroborating | GTPv2 tolerates an unsupported long EAR value by consuming/reporting it safely rather than allowing the unexpected length to derail surrounding decoding. Reinforces resilient length/error handling. |
| !14255 | merged, master | **Deep / promoted; Guy Harris authored and merged** | Adds `PI_RECEIVE` for packet-reception indications and `PI_INTERFACE` for interface/device indications, and carries the taxonomy through WSLua/documentation-facing definitions. Promoted to `expert-info-taxonomy-conventions.md`; semantic group and severity are separate axes. |
| !14254 | merged | Scanned | Extcap argument-setting ownership/leak fix. Resource-lifetime cleanup already covered by stronger notebook guidance. |
| !14253 | merged | Scanned | FAQ URL maintenance. No cross-cutting convention. |
| !14252 | merged | Scanned | Sysdig event parameter dissection now occurs only when parameters are present. Narrow defensive fix. |
| !14251 | merged | Scanned | Miscellaneous Logray NSIS fixes. Packaging maintenance; no additional general rule beyond the stronger !14261 lesson. |
| !14250 | merged | Scanned / corroborating | Extends dissector checker spelling/comment handling. Supports existing checker/tooling conventions. |
| !14249 | merged | Corroborating | WSLua UInt64-to-boolean field support aligns scripting with the widened boolean value domain. Closely related to !14245; no separate rule. |
| !14248 | merged, master | **Deep / promoted** | John Thacker makes first-pass TShark actively service queued asynchronous DNS work and protects dequeueing with a mutex. Promoted with !14244 to `async-work-lifecycle-conventions.md`: a latency-hiding phase must actually service its async queue, with explicit synchronization when multiple contexts can mutate it. |
| !14247 | merged | Scanned | Version-information maintenance. No reusable convention. |
| !14246 | merged | Scanned | Adds Logray profiles to Windows packaging. Corroborates packaged-resource coverage. |
| !14245 | merged, master | Deep / corroborating | Widens `proto_tree_add_boolean*()` value parameters to 64 bits and removes the private special-case boolean64 helper so callers and bitmask paths share one exact-width API. Reinforces exact type/storage-width and API-normalization guidance already present. |
| !14244 | merged, master | **Deep / promoted** | John Thacker fixes two-pass TShark name resolution by draining queued/in-flight asynchronous c-ares work before switching to synchronous second-pass consumption; otherwise an outstanding-cache marker could cause immediate false failure. Promoted to `async-work-lifecycle-conventions.md`. |
| !14243 | merged | Scanned | macOS signing recognizes plugin `.so` extensions. Platform packaging detail only. |
| !14242 | merged | Scanned | Windows libssh dependency update. Dependency maintenance. |
| !14241 | merged | Scanned | Spelling/comment cleanup. |
| !14240 | merged | Scanned | Logray NSIS installer fix. Packaging maintenance. |
| !14239 | merged | Scanned | Logray naming/UI maintenance. |
| !14238 | merged, stable | Scanned / down-weighted | Stable cherry-pick of !14236; corroborates the accepted master fix but adds no independent convention. |
| !14237 | merged | Scanned | Falco bridge tap-data allocation/ownership adjustment. Existing lifetime guidance is stronger. |
| !14236 | merged, master | Scanned | ASAM CMP Vendor Data length handling and UDP encapsulation support. Protocol-specific. |
| !14235 | merged | Scanned | GTP PDU Session Container improvements. Protocol-specific. |
| !14234 | merged | Corroborating | Exposes RTP's generated extended timestamp in the tree/tap-facing data path, providing the canonical value later consumed by !14229. |
| !14233 | merged | Scanned | Spelling correction. |
| !14232 | merged | Scanned | SRT documentation update. |
| !14231 | merged, stable | Scanned / down-weighted | Stable RTP dynamic OPUS clock-rate backport. Accepted bug fix but no independent convention. |
| !14230 | merged | Scanned | CMake/falcodump symlink fix. Build-system maintenance. |
| !14229 | merged, master | **Deep / promoted** | John Thacker removes a second, subtly different RTP timestamp-wrap calculation from RTP Analysis and consumes the dissector's already-normalized 64-bit `info_extended_timestamp` from tap data instead. Tested against wrap/reordering edge cases. Promoted to `tap-data-contract-conventions.md`. |
| !14228 | merged | Scanned | Falco bridge/falcodump fixes. No new cross-cutting rule. |
| !14227 | merged | Scanned | Adds/adjusts spelling-checker comment handling. Tooling maintenance. |
| !14226 | merged | Scanned | Qt About-box Logray naming. UI maintenance. |
| !14225 | merged, master | **Deep / corroborating** | Large first-contribution PLDM FRU dissector received extensive review: field/value naming and `value_string` conventions, Coverity findings, removal of proprietary/OEM material, and requests for a real capture when custom version parsing was hard to assess. The author attached a sample and investigated an existing BCD helper before explaining why its semantics differed. Strongly corroborates existing sample-capture and reuse-existing-helper review guidance; no duplicate rule added. |
| !14224 | merged | Scanned | MATE examples update. Documentation/examples only. |
| !14223 | merged | Scanned | DOCSIS 4.0 MMM preparatory work. Protocol-specific. |
| !14222 | merged | Corroborating | Moves RTMPT debugging to the shared `ws_log` facility instead of local compile-time debugging. Supports using project logging/reporting facades; stronger general API guidance already exists. |
| !14221 | merged | Scanned | Adds RLC/MAC USF information to the Info column. Presentation improvement. |
| !14220 | merged, master | Discussion-focused / corroborating | SANE request/response opcode tracking moves toward conversation/per-frame state, was tested on multiple captures, and discussion sketches a richer frame/timestamp mapping for reciprocal linkage and response time. Reinforces existing transaction/state modeling rather than creating a new rule. |
| !14219 | merged | Scanned | GSUP PDP address naming/dissection changes. Protocol-specific. |
| !14218 | merged | Corroborating | BLF handles zero-length log containers without losing parser progress; includes a concrete reproducer and backport discussion. Reinforces minimum-progress/zero-length parser guidance. |
| !14217 | merged | Corroborating | ISAKMP support for a China IPSec VPN specification included an attached sample capture. Reinforces the established expectation for sample pcaps with protocol/dissector changes. |
| !14216 | merged, master | **Deep / corroborating** | RTCP Profile Specific Extensions move behind a dissector table so Microsoft-specific parsing no longer claims every profile extension and marks unrelated formats malformed. Strongly corroborates the existing rule to use dissector tables for extensible protocol-layer dispatch. |
| !14215 | merged | Scanned | Falco trailing-internal-event handling. Narrow feature fix. |
| !14214 | merged | Scanned | Logray version bump. |
| !14213 | merged | Scanned | Logray default Syscalls profile adjustment. Product/UI default only. |

## Notebook changes promoted from this run

1. `expert-info-taxonomy-conventions.md` — expert group should represent the semantic origin of the condition, with severity as a separate axis; extend public bindings/docs when extending the taxonomy (!14255). Commit `2de70fc784ebf4554fd0ea28bd6566f40c6246b9`.
2. `async-work-lifecycle-conventions.md` — drain outstanding asynchronous work before a synchronous phase assumes completion, and actively service async queues during the phase intended to overlap their latency (!14244, !14248). Commit `3a3e1b94a675d6dddd838670c2c2b7ae7dfef0fa`.
3. `protocol-identity-key-conventions.md` — choose identifiers whose uniqueness scope covers all downstream consumers; use a wider or composite identity when local IDs may repeat (!14262). Commit `e79378e4570108ad42a531e0ce0b7576d9d2337b`.
4. `tap-data-contract-conventions.md` — compute canonical normalized protocol values once at the authoritative dissector layer and reuse them in tap consumers instead of independently reconstructing them (!14229, with !14234 as precursor evidence). Commit `f8d08e6c2f8c79c1a3234f184c1668c983484f45`.
5. `packaging-data-conventions.md` — adding a runtime resource requires auditing every installer/package manifest that explicitly lists its peers, not only the runtime index/build tree (!14261). Commit `884baa75fc3e79ecd933e4d7aed4a72785179600`.
6. `abi-compatibility-conventions.md` — distinguish binary ABI from source-level const qualification and semantic mutation contracts; adding read-only pointee `const` is not inherently an ABI break (!14257, with Guy Harris's direct feedback given especially high weight). Commit `614455614a4fc6e63b32afa1ae2c044fd583ad04`.

The other substantive items in the batch were retained as corroboration rather than duplicated into the notebook where stronger existing rules already cover them: !14216 for dissector-table dispatch, !14245/!14249 for exact-width boolean APIs, !14225/!14217 for sample-capture-supported dissector review, !14220 for transaction state, !14218 for parser progress, and !14259 for localized malformed-input diagnostics.

## Continuity

`mr_14212.json` exists at this corpus commit and is merged, so the corpus is **not exhausted**. If no newer previously-unreviewed MRs are added before the next run, the next descending candidate is **!14212**. The scraping tool therefore does not need to be restarted.