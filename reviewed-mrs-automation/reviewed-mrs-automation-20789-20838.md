# Automated Wireshark MR review — !20789 through !20838

Corpus revision: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed set from the available `reviewed-mrs-automation/` ledgers and sparse `reviewed-mrs.md` tracking, preserving and counting the historical !17571-!17620 batch. The prior run ended at !20839. All fifty corpus IDs !20838 down through !20789 are present, and no pre-existing tracking entry was found for any of them, so these are the fifty highest-numbered previously unreviewed MRs in this corpus revision.

Weighting: merged master MRs and direct maintainer-authored/reviewed rationale receive strongest weight. Stable-branch backports provide confirmation of accepted behavior. Open, closed, draft, abandoned, duplicate, or superseded work is retained in this audit set but down-weighted as precedent.

## Exact reviewed set

- !20838 — Deep, merged master. Wireless-toolbar center-frequency fix adapted logic from `iw`. John Thacker explicitly required preservation of the compatible ISC licensing using Wireshark's normal file-level `SPDX-License-Identifier: ISC` treatment and pointed to existing project source as the model. Promoted to `source-licensing-conventions.md`.
- !20837 — Deep, merged master; authored and merged by John Thacker. Decode-As reporting must accept a valid NULL current handle after a user selects no dissection rather than asserting. Master source for the behavior later seen in stable backport !20840; corroborates existing assertion/precondition guidance.
- !20836 — Discussion-focused, merged master; authored by John Thacker. `wmem_map_reserve()` must report reserved capacity without reading deferred/uninitialized backing-table state. Useful API/data-structure invariant evidence; no separate notebook rule promoted.
- !20835 — Scanned, merged. CAPWAP converts repeated bit-field additions to `proto_tree_add_bitmask_list`; established helper-use cleanup, no new cross-cutting rule.
- !20834 — Scanned, merged. Automatic assigned-number/translation update; no durable engineering lesson.
- !20833 — Scanned, merged automatic update attempt. `manuf` generation failed in this update series; superseded by the successful automatic update, so no implementation precedent extracted.
- !20832 — Scanned, merged automatic update attempt. Same update-series context as !20833/!20834; no separate durable rule.
- !20831 — Deep, merged master; approved by Guy Harris. Fixes inconsistent pcapng handling where size computation wrapped a >65535-byte option while the writer omitted it, producing invalid block lengths. Guy's follow-up explicitly requires any future truncation to preserve UTF-8 code-point boundaries. Strong corroboration of existing UTF-8-safe truncation/output-consistency guidance.
- !20830 — Discussion-focused, merged master; authored by John Thacker. MySQL captures can omit the negotiation handshake, leaving backward-incompatible capabilities unknowable; accepted solution exposes preferences for explicit assumptions rather than pretending the missing state can always be inferred. Protocol-specific application of configurable assumptions.
- !20829 — Scanned, merged release-4.2 backport; authored by John Thacker. Corrects `tvb_get_fle()` usage: the final parameter is an optional NULL-length indicator pointer, not an encoding. Backport confirmation of an API-contract correction.
- !20828 — Scanned, merged companion/master MySQL `tvb_get_fle()` call correction. Same API-contract lesson as !20829; no duplicate notebook rule.
- !20827 — Scanned, merged. MySQL correctly handles `CLIENT_PLUGIN_AUTH_LENENC_CLIENT_DATA`; protocol-specific capability-dependent framing correction.
- !20826 — Scanned, merged. MySQL capability-table/default updates including MariaDB capability support; protocol-specific.
- !20825 — Discussion-focused, merged. ICMP echo payload is always made available to a subdissector and timestamp identification can be suppressed; useful dispatch/fallback behavior but no new general convention.
- !20824 — Scanned, merged. BIST-ITCH cleanup and header/include simplification; no new durable lesson.
- !20823 — Scanned, merged. Const-ifies ISO8583 static type descriptions, moving immutable data into read-only storage; routine const-correctness improvement.
- !20822 — Discussion-focused, merged. USB Audio MIDIStreaming element support was submitted with sample capture material; corroborates the established expectation for representative captures with new dissector functionality.
- !20821 — Discussion-focused, merged. 5co-legacy cleanup restructures script-generated registration code to work better with Wireshark source analysis and registration conventions; corroborates checker/tool-friendly generated-code practices.
- !20820 — Deep, merged master after substantial review. New Bachmann M-Module wiretap/dissector support; Anders Broman explicitly requested an M-Module file to verify the implementation. Strong corroboration of representative sample-file/testing expectations for new capture formats and dissectors.
- !20819 — Scanned, merged. Falco gcpaudit JSON plugin marking plus an array-index guard; straightforward defensive fix.
- !20818 — Scanned, merged. IEEE 802.11 neighbor-report update to current standard; protocol-specific.
- !20817 — Scanned, merged. Zigbee management-routing-response update; protocol-specific.
- !20816 — Discussion-focused, open in corpus snapshot. Proposed signed `INVALID` edge-case correction; retained as provisional evidence only and not promoted as accepted precedent.
- !20815 — Scanned, merged. Zigbee Level Control/Reporting dissection additions and field updates; protocol-specific.
- !20814 — Scanned, merged. Zigbee R23/Zigbee Direct dissector update; feature-specific.
- !20813 — Scanned, merged. Zigbee security NULL-dereference fix; defensive pointer handling consistent with existing malformed/context-state guidance.
- !20812 — Scanned, merged. Zigbee ZCL tunneling GPD command dissectors; feature-specific.
- !20811 — Scanned, merged. GLib compatibility implementation for `g_queue_clear_full`; compatibility maintenance, no new general rule.
- !20810 — Discussion-focused, merged. Dissector-warning fixes reviewed by Martin Mathieson; reinforces treating checker findings as semantic issues to inspect rather than purely mechanical edits.
- !20809 — Scanned, merged. `checkAPIs.pl` console errors highlighted more clearly; tooling usability only.
- !20808 — Scanned, merged. Broad Wiretap const-correctness cleanup; no new convention.
- !20807 — Discussion-focused, merged. Combined Zigbee updates including tunneling and Zigbee Direct fixes; accepted successor/context for the closed narrower attempt !20806.
- !20806 — Scanned, closed/superseded. Earlier standalone Zigbee tunneling submission; down-weighted in favor of accepted subsequent work.
- !20805 — Scanned, merged. Diameter frees memory returned by `xmlNodeGetContent()` with the matching libxml allocator; corroborates allocator-family/ownership discipline.
- !20804 — Scanned, merged. Removes unused hand-rolled PROFINET XML helper; cleanup favors standard maintained parsing paths over dead custom machinery.
- !20803 — Scanned, merged. Initializes generated 5co HFID values consistently with the new default; generated-registration compatibility cleanup.
- !20802 — Scanned, merged/successor in BACnet closing-tag fix series. Protocol-specific correction; no cross-cutting rule.
- !20801 — Deep, merged master; authored by John Thacker. Unknown sFlow flow/counter formats retain their declared length, expose opaque bytes with Expert Info, and advance by padded record extent so following records remain synchronized. Identified as the master source behind stable backport !20860 and added to `parser-boundary-conventions.md`.
- !20800 — Scanned, earlier BACnet closing-tag fix in the same issue series. Down-weighted relative to the accepted later form; no separate lesson.
- !20799 — Scanned, accepted Qt About-dialog copy-row separator correction. User-facing tabular output should use the intended single tab so spreadsheet paste remains column-aligned; narrow UI behavior.
- !20798 — Scanned, merged. Falco k8saudit JSON-plugin support plus NULL guard; straightforward defensive feature fix.
- !20797 — Deep, merged. Continues replacing ambient `wmem_packet_scope()` use with explicit `pinfo->pool`/context passing. Strong corroboration of existing explicit allocator-scope guidance.
- !20796 — Scanned, merged. Makes Qt JSON data-source highlighting support both key+value and value-only ranges; UI/data-source feature.
- !20795 — Discussion-focused, merged. Adds WebSocket support over HTTP/2 and HTTP/3 and shared header tracking. Later !21063 corrected lifetime/ownership problems in this area, so this MR is useful historical architecture context but is not used alone as lifetime precedent.
- !20794 — Scanned, merged. BACnet abbreviation correction; no general lesson.
- !20793 — Discussion-focused, merged. gsm_sim GET RESPONSE handling and APDU reassembly; protocol-specific reassembly work.
- !20792 — Deep, merged master; authored and merged by John Thacker. XML namespace state shallow-copies an epan-scoped name, so freeing that storage after registration leaves a dangling alias, while an independently allocated GLib list still must be freed. Promoted to `allocator-scope-conventions.md` as a shallow-copy lifetime rule.
- !20791 — Scanned, merged. CAPWAP QoS message-element dissection; feature-specific.
- !20790 — Scanned, earlier About-dialog single-tab copy-row change in the same UI fix series; down-weighted relative to the accepted later submission.
- !20789 — Deep, merged master; authored by John Thacker and merged by Michael Mann. Threads real `packet_info *` through NFS/RPC helpers instead of passing NULL, supporting APIs that require packet context and explicit allocator scope. Corroborates explicit-context/allocator guidance.

## Durable notebook changes from this run

- `source-licensing-conventions.md` — new notebook file: copied/adapted third-party code must retain compatible licensing/attribution and use Wireshark's established file-level SPDX treatment; based on direct John Thacker review in merged !20838.
- `allocator-scope-conventions.md` — shallow structure copies retain pointer lifetime obligations; do not free referenced scoped storage while surviving aliases still use it, while still cleaning independently owned allocations; based on merged John Thacker !20792.
- `parser-boundary-conventions.md` — identifies merged master !20801 as the source change for unknown sFlow record synchronization and !20860 as its stable backport, strengthening the existing rule with master+stable provenance.

Existing notebook guidance was strongly corroborated rather than duplicated for valid nullable Decode-As state (!20837), UTF-8-safe truncation and writer/size consistency (!20831), representative sample captures/files (!20822, !20820), explicit packet allocator/context propagation (!20797, !20789), allocator-family matching (!20805), and checker/static-analysis semantics (!20810). Open !20816 and closed/superseded !20806 were deliberately down-weighted.