# Wireshark MR review findings 5111-5160

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

| MR | Outcome | Finding |
|---|---|---|
| !5160 | merged release-3.6 | Gryphon stable backport: test actual proto-data presence rather than inferring initialization from `visited`; corroborates master !5153. |
| !5159 | merged release-3.6 | Automatic generated-data, translation, and release refresh; no new durable rule. |
| !5158 | merged release-3.4 | Automatic generated-data/release refresh; no new durable rule. |
| !5157 | merged master-3.2 | Automatic generated-data/release refresh; no new durable rule. |
| !5156 | merged master | Automatic registry/translation/release refresh; provenance only. |
| !5155 | merged release-3.4 | Focused MKA padding backport; demonstrates the backport value of the !5115 split. |
| !5154 | merged release-3.6 | Focused MKA padding backport; corroborates !5115. |
| !5153 | merged master | John Thacker master origin: look up Gryphon proto-data and create only if absent; `visited` is not proof a nested dissector ran. |
| !5152 | merged master | Removes CI-local `XDG_CONFIG_HOME` workaround after !5129 centralizes it in the shared fixture. |
| !5151 | merged master | John Thacker adds `wmem_multimap_t` for reusable IDs plus frame history; Jaap Keuter requests and receives wmem documentation. |
| !5150 | merged release-3.6 | Column bounds stable fix: reject `column == num_cols`. |
| !5149 | merged release-3.6 | GRegex documentation-link maintenance. |
| !5148 | merged master | GLib documentation-link maintenance. |
| !5147 | merged release-3.6 | Stable backport of BTLE reassembly fix !5140. |
| !5146 | merged master | Packet-list off-by-one fix: model indices are in `[0,num_cols)`. |
| !5145 | merged master | Qt 6 migration review shows configure success is not compile success; macOS and Visual Studio exposed platform-specific requirements. Transitional evidence only. |
| !5144 | merged release-3.6 | Release-note cleanup, including display-filter syntax changes. |
| !5143 | merged release-3.6 | Stable backport of BBLog skipped-block fix !5142. |
| !5142 | merged master | BBLog custom blocks are dispatched by explicit type; skipped/unknown blocks are not treated as event blocks. |
| !5141 | closed | Duplicate release-note restoration, superseded by merged !5138. |
| !5140 | merged master | BTLE reassembly carries advertiser identity into chained fragments and guards subdissection on successful reassembly. |
| !5139 | merged release-3.4 | Stable backport of !5135 5GS TAC/API fix. |
| !5138 | merged master | João Valverde keeps a display-filter deprecation note because users still need migration information at the syntax transition. |
| !5137 | merged release-3.6 | Stable backport of !5135. |
| !5136 | merged master | Moves standard RTPS locator handling out of an RTI-only path into common v2 handling. |
| !5135 | merged master | Pascal Quantin consults a known custom-dissector consumer before changing `dissect_gtpv2_tai()`; additive API was the fallback. |
| !5134 | merged release-3.6 | Stable backport of shared test-environment fix !5129. |
| !5133 | merged release-3.6 | Failed child spawn could close fd 0 because zero-initialized descriptor was cleaned before success; cleanup is delayed. |
| !5132 | closed | Draft direction-key fix for bidirectional GATT discovery; lower-weight history only. |
| !5131 | merged master | Corrects Qt capture-option state mixups among interval/duration/autostop values. |
| !5130 | merged master | Gerald Combs replaces leaking `g_strsplit` with packet-scope `wmem_strsplit`. |
| !5129 | merged master | Gerald Combs removes inherited `XDG_CONFIG_HOME` in the common test environment because it overrides the temporary test home. |
| !5128 | merged master | MKA Announcement parsing remains a separate feature after !5115 split out the stable-worthy padding fix. |
| !5127 | merged master | Uses a Python raw string for expected packet text to avoid invalid-escape warnings. |
| !5126 | merged master | Alexis La Goutte catches field encoding/type use; accepted MMRP fields use semantic field types/encodings. |
| !5125 | merged master | Pascal Quantin enforces standards terminology/capitalization and catches a copied TD-SCDMA filter abbreviation that still said GSM. |
| !5124 | merged release-3.6 | CI publishes a display-filter field list artifact. |
| !5123 | merged release-3.4 | Fixes version text for the display-filter list artifact. |
| !5122 | merged master-3.2 | Release version bookkeeping. |
| !5121 | merged release-3.4 | Release version bookkeeping. |
| !5120 | merged release-3.6 | Temporarily disables WOWW fixed-port registration rather than mis-dissect ordinary HTTP on port 8085. |
| !5119 | merged master-3.2 | Release preparation/bookkeeping. |
| !5118 | merged release-3.4 | Release build/bookkeeping. |
| !5117 | merged master | Expert Info group labels use registered field identity instead of mutable first-entry summary text. |
| !5116 | merged master | Spelling cleanup renamed registered fields; later compatibility guidance is stronger, so those renames are not current precedent. |
| !5115 | merged master | Jaap Keuter requires bug fix and new parsing feature to be separate so the fix can be backported; !5154/!5155 prove the value. |
| !5114 | merged master | macOS build-tool version refresh. |
| !5113 | merged release-3.4 | CI-local XDG workaround; superseded by shared fixture !5129 and cleanup !5152. |
| !5112 | merged release-3.6 | Same temporary CI-local workaround; superseded by !5129/!5134. |
| !5111 | merged master | Jörg Mayer preserves Wireshark's own CMake module path because Qt 6 mutates global `CMAKE_MODULE_PATH`. |

Strongest reusable findings: !5151, !5153, !5129/!5152, !5115, !5135, !5125, !5133, and !5111. Closed !5141 and !5132 are not implementation precedent. There was no substantive Guy Harris technical review in this batch.
