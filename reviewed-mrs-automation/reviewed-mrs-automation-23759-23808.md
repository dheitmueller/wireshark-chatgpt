# Wireshark MR Review Automation: !23759–!23808

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Before selecting this batch, the already-reviewed set was rebuilt from all available tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md`, the aggregate automation tracking where applicable, and the per-run files under `reviewed-mrs-automation/`. Individual MR entries were used rather than assuming that numeric filename ranges implied complete coverage. The historical !17571–!17620 batch remains explicitly preserved and counted.

Exactly 50 previously unreviewed corpus MRs were reviewed, in descending order. The exact set is:

!23808, !23807, !23806, !23805, !23804, !23803, !23802, !23801, !23800, !23799,
!23798, !23797, !23796, !23795, !23794, !23793, !23792, !23791, !23790, !23789,
!23788, !23787, !23786, !23785, !23784, !23783, !23782, !23781, !23780, !23779,
!23778, !23777, !23776, !23775, !23774, !23773, !23772, !23771, !23770, !23769,
!23768, !23767, !23766, !23765, !23764, !23763, !23762, !23761, !23760, !23759.

## Review weighting and findings

- **!23808 — Scanned, merged.** Adds a GLib compatibility definition for `G_REGEX_MATCH_DEFAULT` so code can use the newer API surface while Wireshark still supports older GLib. Straightforward compatibility maintenance; no new cross-cutting rule.
- **!23807 — Deep/corroboration, merged.** John Thacker replaces an OPC UA helper whose length out-pointer contract was being violated with the simpler helper actually needed by the caller. Reinforces choosing the API whose required outputs match caller intent and resolving real static-analysis findings without dummy pointers.
- **!23806 — Deep/corroboration, merged.** John Thacker handles a theoretically overflowing unsigned-timestamp delta whose display result is nonessential. Useful signed/unsigned arithmetic evidence, but existing arithmetic-safety guidance already covers the general rule.
- **!23805 — Scanned, merged release-4.6 backport.** Backports the Plot action lifetime/capture fix from !23795; weighted below the merged master change.
- **!23804 — Scanned, merged.** ASAM CMP 1.1 protocol support; substantial protocol-specific decoding but no distinct cross-cutting review convention.
- **!23803 — Scanned/corroboration, merged.** John Thacker removes ineffective `MAX(0, ...)` and redundant `MIN(...)` operations on unsigned values after Coverity analysis. Reinforces semantic-type reasoning already captured in the notebook.
- **!23802 — Scanned, merged.** Aggregation-view GUI improvements; no new general convention identified.
- **!23801 — Discussion-focused, merged.** BLF writer supplies a reasonable file-start time for interoperability with timeline/replay tools. Discussion notes possible future propagation of capture first/last timestamps; useful format/interoperability context but not a sufficiently broad rule for promotion.
- **!23800 — Scanned, merged.** Lua compatibility selects `lua_closethread` when available to track the newer Lua API while retaining older-version support. No new convention beyond compatibility-layer guidance.
- **!23799 — Scanned, merged.** RDM updates for newer E1.20/E1.37-5 definitions; protocol registry maintenance.
- **!23798 — Scanned, merged.** Adds NATS Decode As support for exact subject names while retaining wildcard matching. Useful protocol extensibility work but no new cross-cutting rule.
- **!23797 — Scanned, merged.** Removes the remaining `ws_debug_printf` API and converts users to `ws_log`; reinforces consolidating on the supported logging abstraction.
- **!23796 — Scanned, merged.** TLS support for ECH configs in RFC 7468 files; feature-specific parsing work.
- **!23795 — Deep, merged master.** Plot actions previously captured a constructor parameter by reference in a triggered lambda; the accepted code stores the flag as object state. Reinforces Qt callback lifetime/ownership rules already represented in the notebook.
- **!23794 — Deep, merged, authored/merged by John Thacker.** SolarEdge cleanup frees temporary GLib data, derives conversation/session crypto state only when needed rather than every redissection, and closes cipher handles via scope cleanup. Strong lifetime/state evidence, but existing allocator/state guidance already covers the durable lesson.
- **!23793 — Scanned, merged stable backport.** Signal-PDU malformed-profile robustness; weighted below the master fix !23765.
- **!23792 — Deep/corroboration, merged.** SolarEdge decrypt output capacity/length fix: the final plaintext is shorter than the intermediate buffer because protocol bytes are removed. Reinforces existing capacity-vs-produced-length guidance.
- **!23791 — Scanned, merged stable backport.** Signal-PDU malformed-profile robustness; weighted below !23765.
- **!23790 — Scanned, merged.** Removes deprecated `tvb_find` routines after migration to newer APIs; no distinct rule beyond API lifecycle cleanup.
- **!23789 — Scanned, merged.** XCSL migration away from deprecated tvbuff search helper; part of the same API cleanup family.
- **!23788 — Scanned, merged.** HL7 migration to `tvb_find_uint16_remaining()`; part of the same bounds-aware API cleanup family.
- **!23787 — Scanned, merged.** O-RAN FH CUS Section Extension 30 updates; protocol-specific.
- **!23786 — Scanned, merged stable backport.** DMX subdissectors receive the payload after the start code; weighted below master !23771.
- **!23785 — Scanned, merged.** Thread/MLE display enhancement for CSL channel page/channel; presentation-specific.
- **!23784 — Scanned, merged stable backport.** IEEE 1722.1 Stream Input Counter correction; weighted below master !23776.
- **!23783 — Scanned, merged.** Corrects Thread CSL Synchronized Timeout parsing typo; protocol-specific bug fix.
- **!23782 — Corroboration, merged stable backport.** Preserves !23778's actionable artifact-download diagnostics in release-4.6.
- **!23781 — Scanned, merged.** Release-note maintenance; no durable engineering convention.
- **!23780 — Scanned, merged.** Conditional compatibility for Lua 5.5's extended `lua_newstate` signature; straightforward version adaptation.
- **!23779 — Scanned, merged.** Adds Aruba vendor-specific AP Health decoding; protocol-specific.
- **!23778 — Deep, merged, authored/merged by Gerald Combs.** `FetchArtifacts.cmake` now preserves the transport error text and download log when a required artifact fetch fails. Promoted to `build-download-diagnostics-conventions.md`: required build downloads should fail loudly with the actionable diagnostics exposed by the underlying API.
- **!23777 — Scanned, merged.** Adds RFC 9925 OIDs; registry/protocol maintenance.
- **!23776 — Scanned, merged master.** Corrects IEEE 1722.1 Stream Input Counters to match the protocol definition; no broader rule.
- **!23775 — Scanned/corroboration, merged.** Coverity-found operator-precedence fix in SolarEdge. Reinforces explicit expression/sequencing guidance already in the notebook.
- **!23774 — Scanned, merged.** Disables an ambiguous SIMtrace extended-length interpretation because command/response direction cannot be reliably distinguished. Protocol-specific heuristic decision.
- **!23773 — Scanned, merged.** Registers SGP.32-specific objects separately from SGP.22; protocol registration maintenance.
- **!23772 — Deep/corroboration, merged.** Updates Coverity annotations to the analyzer's current primitive name. Reinforces the existing rule that analyzer models/annotations should be corrected when established code semantics are sound.
- **!23771 — Scanned, merged master.** DMX passes a subset excluding the start code to subdissectors. Reinforces the existing subset-TVBuff/protocol-boundary design rule.
- **!23770 — Scanned, merged.** Adds missing Darwin pcapng option values; file-format table maintenance.
- **!23769 — Scanned, merged.** ASAM CMP padding detection prevents Ethernet padding from being decoded as another CMP PDU. Protocol-specific framing correction.
- **!23768 — Deep, merged.** After `tvb_find_line_end_remaining()` changed from returning a length to returning `bool` with the length in an out-parameter, MGCP/MSN callers accidentally treated the status as a length and obtained 0/1. Tamas Regos explicitly reviewed the accepted pattern: consume the value from the out-parameter and explicitly `(void)` the status only where surrounding logic intentionally makes it unnecessary. Promoted to `parser-api-status-outparam-conventions.md`.
- **!23767 — Deep, merged.** QCDIAG removes libxml2 `XML_PARSE_NOENT` after Coverity identifies it as an unsafe parser configuration vulnerable to external-entity expansion. Promoted to `xml-parsing-security-conventions.md`. Michael Mann also required the commit/MR summary to describe the general improvements actually included; this corroborates existing submission-scope guidance rather than creating a duplicate rule.
- **!23766 — Deep/corroboration, merged.** YAMI validates packet-derived lengths before `tvb_get_string_enc()` after Coverity taint findings. Reinforces existing hostile-length/bounds guidance.
- **!23765 — Deep/corroboration, merged master.** Signal-PDU handles illegal profile combinations without aborting Wireshark. Reinforces recoverable malformed-input handling rather than programmer assertions/crashes.
- **!23764 — Scanned, merged stable backport.** Backport of generated `value_string` termination fix from !23763.
- **!23763 — Deep/corroboration, merged master.** Ensures a generated Darwin `value_string` has the required terminating entry. Reinforces sentinel/table invariants; no new rule needed.
- **!23762 — Deep, merged, with John Thacker rationale.** TVB backing bytes cannot live on a helper's stack because consumers can use the TVB after that dissector returns; accepted code allocates from `pinfo->pool`. Strongly corroborates existing allocator/lifetime guidance, so no duplicate convention was added.
- **!23761 — Scanned, merged.** Adds a GLib compatibility implementation for `g_hash_table_steal_extended`; compatibility-layer maintenance.
- **!23760 — Scanned, merged.** Adds the required `errno` include after a build failure; straightforward portability/build correction.
- **!23759 — Deep/corroboration, merged release-4.4 backport.** Reassembled BT HCI ISO TVBs use captured length equal to bytes actually initialized (`cur_off`) while retaining the logical/report length separately, preventing consumers from seeing uninitialized memory. Strongly reinforces existing captured-vs-reported-length and reassembly guidance.

## Durable notebook promotions

- `xml-parsing-security-conventions.md`: do not enable external-entity substitution such as libxml2 `XML_PARSE_NOENT` for untrusted XML; remove the unsafe parser capability rather than trying to sanitize after expansion (!23767).
- `parser-api-status-outparam-conventions.md`: when an API separates status from value out-parameters, preserve those domains during migration; a status return must not be reused as the old numeric result, and intentionally ignored status should be explicit (!23768).
- `build-download-diagnostics-conventions.md`: required artifact downloads should surface the underlying error and transfer log when they fail, because actionable failure diagnostics are part of build reliability (!23778, corroborated by !23782).

Merged master changes were weighted most heavily. Stable backports were used primarily as corroboration. John Thacker's authored/merged correctness and lifetime work, Gerald Combs's build-system change, Michael Mann's direct submission feedback, and explicit reviewer reasoning in !23768 received elevated weight. No abandoned proposal in this batch was used to override merged evidence.
