# Wireshark MR automation review: !13262 through !13213

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook repository: `dheitmueller/wireshark-chatgpt`
- Selection direction: descending from the highest-numbered MR not already present in the review tracking.
- Batch size: 50 MRs (maximum requested).
- Status summary: 50 merged MRs; no closed/unmerged or open MRs in this batch.

## Selection/reconciliation

Before selecting the batch, the automation reconciled the available per-run files in `reviewed-mrs-automation/` with `reviewed-mrs.md` and other aggregate tracking. MR numbers were treated individually rather than assuming that a numeric interval was fully reviewed. The historical reviewed batch !17571-!17620 remains preserved and counted. The preceding ledger reviewed through !13263 and explicitly identified !13262 only as an unreviewed frontier probe, so !13262 remained eligible for this run.

## Exact reviewed MR set

The following MR numbers, and only these MR numbers, were counted as reviewed in this run:

- !13262
- !13261
- !13260
- !13259
- !13258
- !13257
- !13256
- !13255
- !13254
- !13253
- !13252
- !13251
- !13250
- !13249
- !13248
- !13247
- !13246
- !13245
- !13244
- !13243
- !13242
- !13241
- !13240
- !13239
- !13238
- !13237
- !13236
- !13235
- !13234
- !13233
- !13232
- !13231
- !13230
- !13229
- !13228
- !13227
- !13226
- !13225
- !13224
- !13223
- !13222
- !13221
- !13220
- !13219
- !13218
- !13217
- !13216
- !13215
- !13214
- !13213

## Durable findings promoted to the notebook

### Shared semantic preconditions belong at the common operation boundary

Merged master !13242 fixes a crash where `tshark -U` could accept a registered protocol/tap that was not an export-PDU tap. John Thacker explicitly suggested moving the suitability test into `exp_pdu_pre_open()` rather than relying on the TShark caller; the final implementation does so before tap registration. Merged release backports !13261 and !13262 preserve the same placement. New `api-precondition-validation-conventions.md` records that syntactic/existence checks do not replace operation-specific semantic validation, and that a common operation should defend its own invariant before side effects.

### Normalize encoded input once and keep opaque data as bytes plus length

John Thacker's merged master !13219 moves WPA passphrase percent-decoding into `parse_key_string()` so constraints are applied to decoded data and highlights the need to preserve embedded NUL bytes. His merged !13233 then stores parsed keys as `GByteArray`, removes repeated text/byte conversion and duplicated validation, and lets downstream consumers use the parser's typed result. Merged !13247 immediately audits a unit consequence of the representation change, correcting bit length from `len * 4` to `len * 8`. New `binary-data-representation-conventions.md` records the parsing-boundary, explicit-length, validation-ownership, and representation-unit rules.

### Parser errors should preserve actionable rejection reasons

Merged master !13243, authored and merged by John Thacker, changes 802.11 key parsing from a generic invalid-format diagnostic to specific reasons such as empty input, non-hexadecimal data, and length violations. `api-error-reporting-conventions.md` now records that the parser which owns the grammar should return the semantic cause rather than forcing frontends to reconstruct it.

### Recent-history startup reads must be non-destructive until effective preferences are known

Merged master !13260, authored by John Thacker, fixes recent-file truncation at startup. `recent_common` must be read before preferences because it can select the last-used profile; therefore applying the default recent-file maximum during that early read can discard entries before the selected profile or command-line preference overrides establish the real limit. `recent-state-and-history-conventions.md` now records the rule to preserve early startup history and apply preference-dependent limits only after configuration precedence is resolved.

### A fallback UI default is not persisted user history

Merged release-4.2 !13213, authored and merged by Guy Harris, separates the real persisted last-open directory from the derived initial directory for an open/save dialog. It also handles macOS Finder launches where cwd may be `/`, falling back to a meaningful personal/home directory rather than treating `/` as user intent. Startup no longer writes the fallback into last-open history simply to initialize a dialog. This high-authority distinction is recorded in `recent-state-and-history-conventions.md`.

### Shared frontend policy should be centralized as a semantic helper

Merged master !13241, authored and merged by Guy Harris, moves file-dialog style and initial-directory policy out of duplicated Qt dialog code into common helpers. `application-layer-boundary-conventions.md` now records that common application/UI code should answer the semantic question directly rather than exposing raw preference/history pieces and requiring each dialog to reconstruct the policy.

### Protocol constraints must follow the specification's full domain, not a common subtype

Merged master !13225 adds DHCPv6 Client Link-Layer Address option handling with a sample capture. John Thacker catches an assumption that effectively required an Ethernet-sized address even though RFC 6939 permits any valid IANA hardware type. The accepted revision validates the fixed hardware-type portion and treats the remaining address length according to the actual option. Release-4.2 !13255 carries the corrected change. `authoritative-source-preservation-conventions.md` now records the general rule against narrowing a specification to its most common subtype.

### Installer runtime dependency inventories should have one source of truth

Merged master !13227 adds the missing minizip runtime DLL to Windows packaging; merged !13231, both authored and merged by Gerald Combs, immediately consolidates duplicate WiX dependency enumerations into one `_dll_list` used for both generated sections. Release-4.2 !13229 and !13232 propagate those fixes. `packaging-conventions.md` now records the single-source runtime-inventory rule.

## Important corroborating material not promoted as duplicate rules

- !13237 and release backport !13238 show why changes must build against the oldest supported Qt API level: `QProcess::splitCommand` was introduced only in Qt 5.15 while Wireshark still supported Qt 5.12, so Peter Wu copied the upstream Qt implementation for compatibility. This corroborates existing build-variant/oldest-supported-version guidance.
- !13226 fixes an RF4CE out-of-bounds copy and includes explicit review by a protocol-domain contributor; it corroborates existing bounded-copy/dissector validation guidance.
- !13253 and !13236 continue source-checker/field-registration cleanup and corroborate existing checker conventions.
- !13234 is Debian exported-symbol maintenance and corroborates existing ABI/symbol-manifest guidance.
- !13235 and !13240 expose follow-on complexity in moving display-filter macros from UAT-backed configuration to profile-aware handling. The later MR still had reviewer uncertainty about default/profile macro merging, so the sequence was retained as useful migration evidence but not elevated into a new high-confidence convention.
- !13257/!13258, authored and merged by Guy Harris, correct HTTP load-statistics result classification (`100 <= status < 400`) and user-facing terminology from “KO” to “Error”; authoritative but narrow.
- !13259 is a Guy Harris comment/documentation clarification about native versus non-native Qt file dialogs; useful context for !13241 but not a separate convention.
- !13248 adds and wires user-guide documentation for 802.11 decryption-key configuration, including UAT Help integration; good documentation hygiene but no additive cross-cutting rule.
- !13249/!13250 are standards-version protocol updates; no review discussion yielded a stronger general rule beyond existing generated/specification guidance.
- !13244/!13245 are release-preparation changes; !13224, !13222, !13221, and !13220 are automatic data updates; these were reviewed and counted but carried little architectural weight.

## Notebook commits from this run

- `6ccc8ce9eae0623c6c78b3bb5b2a66642a3a452a` — add non-destructive early recent-history handling from !13260 and Guy Harris's history-versus-dialog-fallback distinction from !13213.
- `c7e4fcc8293b597c67f1128902d1a0c64b6acd43` — add shared API semantic-precondition placement from !13242/!13261/!13262.
- `4107b9b153be35d0cf2b68d3a4456e10162c61a6` — add binary input representation, normalization, explicit-length, and representation-unit rules from !13219/!13233/!13247.
- `b643910d05c1b6b9a992a02ec467845f72cc0549` — add actionable parser-error guidance from !13243.
- `d3fb82520a5c2d8cd2b0db8e21f20d5365bf3da8` — add single-source Windows installer runtime-dependency inventory rule from !13227/!13231 and release backports.
- `be438681611667c0a9e6474df31a4fd4de13111e` — add common frontend-policy helper guidance from Guy Harris's !13241.
- `a315b3e4756d85f9070b550ce8b7613e9cc55570` — add specification-domain constraint guidance from !13225/!13255.

## Frontier

After completing the 50-MR batch, !13212 (`dot11decrypt: Fix setting log domain`) was fetched only to verify that the corpus continues. It is merged and was **not** counted as reviewed in this run. Therefore the corpus is not exhausted; absent newly scraped higher-numbered unreviewed MRs, !13212 is the next descending candidate.