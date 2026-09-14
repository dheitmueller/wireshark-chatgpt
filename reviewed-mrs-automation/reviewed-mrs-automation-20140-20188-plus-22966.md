# Wireshark MR review automation ledger — !22966 plus !20188-!20140

Corpus commit reviewed: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

This run reviewed exactly 50 previously unreviewed merge requests. Selection was rebuilt from `reviewed-mrs.md` and the per-run ledgers in `reviewed-mrs-automation/`, using individual MR IDs rather than assuming numeric intervals were complete. The historical !17571-!17620 batch remains part of the already-reviewed set.

A new corpus commit landed after the previous run. It filled the previously absent !22966 hole, which therefore outranked the existing descending frontier below !20189. The other historical absent IDs explicitly recorded in prior ledgers (!21132 and !21133) remain absent in this corpus snapshot. Previously skipped IDs such as !22662 and !22208 were confirmed to have already been reviewed and were not revisited.

## Exact MRs reviewed

`!22966 !20188 !20187 !20186 !20185 !20184 !20183 !20182 !20181 !20180 !20179 !20178 !20177 !20176 !20175 !20174 !20173 !20172 !20171 !20170 !20169 !20168 !20167 !20166 !20165 !20164 !20163 !20162 !20161 !20160 !20159 !20158 !20157 !20156 !20155 !20154 !20153 !20152 !20151 !20150 !20149 !20148 !20147 !20146 !20145 !20144 !20143 !20142 !20141 !20140`

Count: **50**.

## Review notes

| MR | Status | Review result |
|---|---|---|
| !22966 | Scanned | Merged Anders Broman DCERPC conversion to unsigned offsets/lengths. Broad API-domain cleanup; corroborates the notebook's existing non-negative offset/length guidance rather than adding a new rule. |
| !20188 | Scanned | Merged John Thacker SSH sequence-number simplification during first KEX. Centralizes sequence accounting in the standard message-info state and fixes DEBUG/IGNORE-before-KEX behavior; corroborates keeping protocol state in the canonical state object. |
| !20187 | Scanned | Guy Harris-authored release-4.2 backport: unsupported Netflix BBLog pcapng block types must produce a read error instead of silently terminating input. Strong corroboration for distinguishing parser failure from clean EOF. |
| !20186 | Scanned | Merged FT_STRING encoding cleanup removing inappropriate `ENC_NA` uses. API correctness cleanup; no new durable convention. |
| !20185 | Scanned | Guy Harris-authored release-4.4/master-line version of the pcapng unknown-BBLog-block error behavior represented by !20187. Same read-error-vs-EOF lesson. |
| !20184 | Scanned | Merged HTTP/2 associated-IMSI enhancement. Protocol/application-specific; no general convention promoted. |
| !20183 | Scanned | Merged WSUG documentation for Reload as File Format. Documentation-only. |
| !20182 | Scanned | Merged JSON-3GPP regex-group correction to the prior SUPI/IMSI work. Narrow correctness fix. |
| !20181 | Scanned | Merged E.212 operator-data update. Data maintenance only. |
| !20180 | Scanned | Merged E.212 test-network data entries. Data maintenance only. |
| !20179 | Discussion-focused (closed) | NHRP IPv6/Ethertype submission closed unmerged after a failed pipeline; down-weighted as non-accepted implementation evidence. |
| !20178 | Scanned | Merged `check_tfs.py` output sorting improvement. Tool usability only. |
| !20177 | Scanned | Merged JSON-3GPP SUPI/associated-IMSI feature. Protocol-specific; later !20182 corrects a regex-group mistake. |
| !20176 | Scanned | Merged GLib compatibility fix for `G_REGEX_DEFAULT` on pre-2.74 systems. Useful portability repair but no new general rule beyond existing compatibility guidance. |
| !20175 | Scanned | Stratoshark version bump. Release bookkeeping only. |
| !20174 | Scanned | CI correction for Windows Arm64 Falco plugin version. Build maintenance only. |
| !20173 | Scanned | Stratoshark release build/version maintenance. No durable engineering lesson. |
| !20172 | Scanned | Stratoshark CloudTrail profile field-name correction. Configuration maintenance. |
| !20171 | Scanned | Merged Falco Events compatibility field restoration as hidden. Compatibility/presentation-specific; no separate rule promoted. |
| !20170 | Discussion-focused (closed) | Earlier NHRP IPv6/Ethertype attempt; closed unmerged and therefore down-weighted. |
| !20169 | Scanned | Merged Qt translation consistency cleanup using the literal ellipsis character. UI/i18n maintenance. |
| !20168 | Scanned | Merged Couchbase opcode additions sourced from upstream protocol definitions. Protocol data maintenance. |
| !20167 | Scanned | Additional merged FT_STRING encoding cleanup. Corroborates correct encoding-domain use. |
| !20166 | Scanned | Merged Diameter session-wide associated-IMSI propagation. Protocol/session presentation feature; no new general state rule. |
| !20165 | Scanned | Merged `packet-silabs-dch` migration from ambient `wmem_packet_scope()` to explicit allocator/pinfo context. Corroborates existing allocator-scope and concurrency-readiness guidance. |
| !20164 | Scanned | Merged SCTP migration from ambient packet scope to `pinfo->pool`. Same explicit allocator-context lesson. |
| !20163 | Scanned | Merged RTSP removal of mutable static stats state plus explicit allocator parameter. Strong corroboration for concurrency-safe/local state and explicit allocator context. |
| !20162 | Scanned | Merged Qt SimpleDialog queueing fix moves secondary-message data into construction/queueing state before deferred display. Lifecycle-specific; no separate notebook rule. |
| !20161 | Deep | Merged tap-wspstat fix. Registers a finish callback, delegates hash ownership to `g_hash_table_new_full()`, and fixes inconsistent integer-key representation that caused real lookup misses (`(null)` instead of `200 OK`). Promoted to `state-representation-conventions.md`. |
| !20160 | Scanned | Merged stats-tree tap finish callback ensuring teardown runs through the tap lifecycle. Corroborates explicit listener cleanup/lifetime ownership. |
| !20159 | Scanned | Merged PFCP session-wide IMSI association, following !20144. Protocol-specific. |
| !20158 | Deep | Merged SSH multi-session and rekey decryption support with extensive John Thacker review. Key durable lesson: do not allocate state for every frame when state changes only at transitions; use per-frame proto data for genuinely per-frame values or a predecessor-searchable `wmem_tree_t` for transitions such as REKEY. Review also corrected SSH sequence-number semantics and the submission supplied multi-session/rekey captures, keylogs, debug output, live output, and second-pass output. Promoted to `state-representation-conventions.md`. |
| !20157 | Scanned | Merged PFCP conversion from `GHashTable` to wmem maps. Container/lifetime cleanup; no independent convention beyond existing wmem ownership guidance. |
| !20156 | Scanned | Qt numerus/plural translation backport. UI/i18n maintenance. |
| !20155 | Scanned | Qt plural/translatable-days backport. UI/i18n maintenance. |
| !20154 | Discussion-focused (closed) | SCSI READ CAPACITY LBA display proposal closed unmerged; its interpretation was not used as accepted architecture evidence. |
| !20153 | Scanned | Merged Coverity-driven BT ATT NULL check. Static-analysis correctness fix; corroborates existing warning/static-analysis guidance. |
| !20152 | Deep | Merged Wiretap refactor shares SocketCAN structures and `wtap_socketcan_gen_packet()` across CAN readers. Together with !20146 it forms an explicit simplify-then-extract-commonality sequence, promoted to `state-representation-conventions.md` as a refactoring pattern. |
| !20151 | Scanned | Merged Falco Events tree/presentation adjustment. UI-specific. |
| !20150 | Scanned | Qt translation cleanup. UI/i18n maintenance. |
| !20149 | Scanned | Qt numerus/plural translation fix. UI/i18n maintenance. |
| !20148 | Scanned | Qt plural/translatable-days fix. UI/i18n maintenance. |
| !20147 | Scanned | Merged O-RAN FH CUS ULPI checks/comments update. Protocol-specific. |
| !20146 | Deep | Merged candump parser simplification from Flex/Lemon to straightforward C, explicitly motivated by making commonality among text CAN wiretap readers easier to expose; paired with !20152 for the promoted refactoring lesson. |
| !20145 | Scanned | Merged Falco Events offset corrections to match SDK/plugin behavior. Implementation-specific. |
| !20144 | Scanned | Merged GTPv2 session-wide associated-IMSI propagation; basis for adjacent PFCP/Diameter work. Protocol-specific. |
| !20143 | Scanned | Guy Harris-authored pcapng handler-signature cleanup removes an unused `pcapng_t *` and aligns block-reader call shape. Good API simplification but no separate rule needed. |
| !20142 | Scanned | Guy Harris-authored pcapng cleanup passes the actual block type instead of a derived EPB Boolean, aligning direct calls with generic block-handler dispatch. Corroborates preserving the authoritative discriminator rather than introducing redundant proxies. |
| !20141 | Deep | Merged preference representation refactor: mutually exclusive preference type becomes an enum while orthogonal obsolescence becomes a separate Boolean. Removes masking-before-switch/equality hazards and aligns Lua/epan semantics. Promoted to `state-representation-conventions.md`. |
| !20140 | Scanned | Automatic data/translation update. No durable engineering-review lesson. |

## Durable notebook changes from this run

Created `state-representation-conventions.md` with four accepted patterns:

1. Model mutually exclusive type and orthogonal attributes separately, from merged !20141.
2. Keep hash-key representation consistent with its hash/equality/ownership contract, from merged !20161.
3. Represent sparse state transitions by their change points rather than capture-wide per-frame arrays, based on direct John Thacker review in merged !20158; also record the associated multi-session/rekey validation practice.
4. When appropriate for a genuinely simple grammar, simplify accidental parser machinery to expose common structure, then extract shared record construction, from merged !20146 followed by !20152.

Guy Harris-authored !20185/!20187 and !20142/!20143 were given high authority but treated as corroboration of already-established parser/API principles rather than duplicated into new rules.

## Selection continuity

After this run, the descending frontier is below **!20140**, except for any higher-numbered holes that a future corpus refresh fills. Future runs must continue reconstructing the reviewed set from exact individual ledger entries; in particular, newly populated historical holes may outrank the apparent contiguous frontier.