# Wireshark MR review findings 5161-5210

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Exactly 50 merge requests are recorded below. Merged master changes are weighted most heavily; stable backports corroborate master behavior; closed submissions are lower-weight history.

| MR | Outcome | Depth | Findings |
|---|---|---|---|
| !5210 | merged master | Scanned | Documents the preferences API with Doxygen. Useful documentation work, but later merged !5217 supplies the stronger maintainability convention for bare `@file` markers, so !5217 remains authoritative for style. |
| !5209 | merged master | Deep | Pascal Quantin removes WebSocket's handoff routine from the preference-apply callback because no preference requires rerunning handoff and repeated execution re-registers the dissector in the TCP table. Master origin of the registration-side-effect rule, with !5218 as stable corroboration. |
| !5208 | merged master | Deep | João Valverde adds a public enum/macro introspection surface for bindings because the C ABI does not expose those constants. The generated C table is checked in; its heavyweight parser/generator is an explicit non-default target. Strong API/bindings evidence. |
| !5207 | merged release-3.6 | Scanned | gRPC returns cleanly for a zero-length message body before attempting compression/body dissection. Straightforward empty-payload hardening. |
| !5206 | merged master | Discussion-focused | Improves Windows version detection using build numbers, but the discussion immediately identifies newer Windows 11 builds that exceed the exact `22000` check. Useful evolution evidence, not a durable exact-version-table precedent. |
| !5205 | merged master | Discussion-focused | Broad QRegExp→QRegularExpression migration. Linux compilation exposed a missing direct include; the author points to follow-up !5247. Corroborates compiling mechanical API migrations across configurations/platforms rather than trusting source-level similarity. |
| !5204 | merged master | Scanned | Adds QUIC v2 draft-00 support and corresponding key-derivation/version handling. Protocol-specific; no new cross-cutting convention extracted. |
| !5203 | merged master | Discussion-focused | Removes an artificial RTPS DataType element cap because complete type metadata is needed for nested-member offsets. Alexis La Goutte challenges malformed-input loop risk and John Thacker catches dead code left by the old limit. Retained as cautionary evidence: correctness-driven cap removal still requires structural termination/resource reasoning. |
| !5202 | merged master | Deep | Jaap Keuter fixes a leak by freeing an error string even when the failure is intentionally suppressed. Error presentation policy and resource ownership are separate contracts. |
| !5201 | merged master | Deep | Cisco ERSPAN marker fixes are validated against real Nexus 9000 captures and vendor documentation. Jaap Keuter rejects a misleading TAI rename, requires commit-subject cleanup, and asks for sample captures; the contributor publishes anonymized samples. Strong review/submission evidence for vendor-specific dissectors. |
| !5200 | merged master | Discussion-focused | Adds Exif large-value and rational/srational handling. Jaap Keuter catches include ordering: `config.h` must remain first. Corroborates existing source-file/build convention. |
| !5199 | merged master | Scanned | Signal-PDU helper fixes its return value to report the parsed offset rather than an unrelated remaining-length expression. Reinforces that consumed-length return contracts must match caller expectations. |
| !5198 | merged master | Scanned | Guards a `tvb_bytes_to_str_punct` call against null tvbuff / zero payload to eliminate a runtime warning. Straightforward defensive API use. |
| !5197 | merged master | Discussion-focused | Fixes standard-vs-extended CAN ID semantics across Signal-PDU and AUTOSAR I-PduM, including UAT validation and dissector-table registration. Review catches the parallel consumer, reinforcing audits of all paths sharing an encoded identifier. |
| !5196 | merged master | Scanned | Frees temporary key lists returned while iterating hash tables in several Signal-PDU registration helpers. Straightforward ownership cleanup. |
| !5195 | merged master | Scanned | Renames internal `proto_reg_handoff_*`-named helpers to `register_*` because they are re-registration helpers, not top-level handoff entry points. Corroborates semantic API/helper naming. |
| !5194 | merged master | Scanned | Centralizes PCRE2 runtime version retrieval and sizes the buffer from `pcre2_config`. No broader new convention beyond using the dependency's sizing API. |
| !5193 | merged master | Scanned | Removes a stale PCRE ownership comment. Maintenance only. |
| !5192 | merged master | Deep | Display-filter scanner frees and clears its partially built quoted string before returning `SCAN_FAILED` on an invalid escape. Strong failure-path ownership evidence for lexers. |
| !5191 | merged master | Scanned | Improves character-constant diagnostics, including an explicit empty-constant error. Part of the broader scanner/literal cleanup culminating in !5187/!5180. |
| !5190 | merged master | Deep / high-authority | John Thacker fixes an RTMPT infinite loop by replacing raw TCP sequence comparison with wrap-aware `GE_SEQ`. Later !5225 shows modular comparison alone can still need a conservative bound when the backing tree assumes linear order; together they define the durable rule. |
| !5189 | merged master | Scanned | Updates stats-tree documentation/sample code to the current return/datatype API and clarifies CLI abbreviation versus GUI display name. Documentation/API synchronization only. |
| !5188 | merged master | Discussion-focused | Pascal Quantin flags a suspicious field type and points to the existing STK dissector for possible nested decoding. Useful reminder to match `hf_` type to wire data and reuse established subdissectors only when the embedded format is actually known. |
| !5187 | merged master | Deep | João Valverde parses character constants in the lexer into a semantic numeric value instead of repeatedly coercing them through generic string/byte conversion. Strong display-filter lexer/semantic-boundary evidence. |
| !5186 | merged master | Discussion-focused | Transitional Windows 10/11 and Server labeling change whose discussion directly motivates !5206. Kept as evolution context, not final version-detection policy. |
| !5185 | merged release-3.6 | Scanned | Adds missing `config.h` includes across many translation units. Stable corroboration of the build convention that generated configuration definitions be available where expected. |
| !5184 | merged master | Scanned | Corrects MACsec capability value strings to match IEEE 802.1X semantics. Protocol-specific mapping correction. |
| !5183 | merged master | Scanned | Adds the explicit `ws_version.h` dependency to the example plugin. Corroborates keeping examples buildable with direct header dependencies. |
| !5182 | merged master | Deep | Gives character constants their own syntax-tree type so invalid constants get character-constant diagnostics rather than generic value-string behavior. Precursor to !5187's lexer-level semantic value. |
| !5181 | merged master | Scanned | Removes an obsolete GRegex label from display-filter VM dump output after the regex backend change. Maintenance only. |
| !5180 | merged master | Deep | João Valverde makes unknown escapes in double-quoted display-filter strings syntax errors, updates the User's Guide and release notes, and adds a regression test. Strong language-compatibility and scanner-validation evidence. |
| !5179 | merged release-3.6 | Scanned | Publishes the generated display-filter field list as a CI artifact. Release/CI plumbing; no new general convention promoted. |
| !5178 | merged release-3.6 | Scanned | 3.6.0→3.6.1 version/release-note reset. Release bookkeeping. |
| !5177 | merged release-3.6 | Scanned | Final 3.6.0 release-note/build updates, including display-filter syntax and packaging notes. Release bookkeeping. |
| !5176 | merged master | Deep | Compacts multi-line packet-comment presentation while retaining the original unsplit comment as a hidden field so filtering/search remains compatible. Strong separation of presentation from semantic/filterable data. |
| !5175 | merged master | Discussion-focused | Changes expert-info add APIs to return the created item so packet-comment expert presentation can be hidden while expert reporting remains available. Jaap Keuter warns about inconsistent per-dissector expert presentation; accepted behavior is useful but should not be generalized casually. |
| !5174 | merged master-3.2 | Scanned | Old-branch EVS bandwidth fix; duplicate implementation evidence for master !5167. |
| !5173 | merged release-3.4 | Scanned | EVS bandwidth fix backport; duplicate of master !5167. |
| !5172 | merged release-3.6 | Scanned | EVS bandwidth fix backport; duplicate of master !5167. |
| !5171 | merged release-3.6 | Scanned | MKA Announcement TLV/cipher-suite parsing backport. Protocol-specific and primarily corroborative. |
| !5170 | merged master | Scanned | Removes unused, obsolete Qt MacExtras dependency/configuration. Straightforward dependency cleanup. |
| !5169 | merged release-3.4 | Scanned | Improves Bluetooth LE advertising-data reassembly by carrying advertiser address state and checking for valid reassembly before invoking the inner dissector. Useful reassembly corroboration. |
| !5168 | merged master | Scanned | Large Bluetooth Mesh sensor/property dissector expansion. Jaap Keuter notes release-finalization priority for such a large review; no new technical convention extracted. |
| !5167 | merged master | Discussion-focused | Pascal Quantin guides the EVS fix so lookup transformation reuses the existing value table while the displayed one-bit field keeps its raw 0/1 value. Good field-semantics discipline, but protocol-specific. |
| !5166 | merged master-3.2 | Scanned | CI image-tag/container-registry maintenance. No durable coding/review convention. |
| !5165 | closed/unmerged | Discussion-focused / high-authority | Guy Harris corrects the MR terminology: field names/abbreviations are not “filters”; a display filter is an expression involving fields, operators, and values. Martin Mathieson closes the MR because the branch accidentally contains unrelated spelling work. Retain the terminology and branch-hygiene lesson, not the unmerged code. |
| !5164 | closed/unmerged | Discussion-focused | Jaap Keuter questions a unique error-message-reset pattern; Anders Broman asks for rebase and later marks it obsolete. No accepted implementation precedent. |
| !5163 | merged master | Scanned | Updates documented macOS minimum according to the Qt 5.15 dependency. Platform-support bookkeeping. |
| !5162 | merged master-3.2 | Deep | Gryphon stops assuming `pinfo->fd->visited` implies its own proto-data exists; it first looks up packet-scoped state and creates it if absent. Prevents segfaults when TCP sequencing means the dissector did not actually see the nominal first pass. |
| !5161 | merged release-3.4 | Deep | Same Gryphon proto-data-existence fix as !5162 on release-3.4. Independent maintained-branch corroboration of the state-presence rule. |

## Batch-level observations

The strongest reusable material is the WebSocket preference/handoff registration fix (!5209), explicit binding introspection API (!5208), vendor-capture review workflow (!5201), RTMPT wrap-aware traversal fix (!5190), display-filter lexer/literal sequence (!5192/!5187/!5180), presentation-versus-filter semantics for packet comments (!5176), and Gryphon's proto-data existence check (!5162/!5161).

Closed !5165 contributes unusually authoritative Guy Harris terminology guidance, but its code is not treated as accepted precedent. Closed !5164 contributes no durable implementation rule.
