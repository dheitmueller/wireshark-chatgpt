# Reviewed Wireshark MRs — automation batch !12163–!12212

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Selection method: reconstructed the already-reviewed set from `reviewed-mrs.md`, the supplemental `reviewed-mrs-automation/reviewed-mrs-automation.md`, and the available per-run ledgers under `reviewed-mrs-automation/`. The previously reviewed !17571–!17620 batch remains part of that set. The prior run's !12212 lookup was a frontier probe only, not a review. No existing review tracking was found for the selected !12163–!12212 MRs, so these are the fifty highest-numbered previously unreviewed MRs available in this corpus snapshot.

Exact reviewed MR set (50):

!12212, !12211, !12210, !12209, !12208, !12207, !12206, !12205, !12204, !12203,
!12202, !12201, !12200, !12199, !12198, !12197, !12196, !12195, !12194, !12193,
!12192, !12191, !12190, !12189, !12188, !12187, !12186, !12185, !12184, !12183,
!12182, !12181, !12180, !12179, !12178, !12177, !12176, !12175, !12174, !12173,
!12172, !12171, !12170, !12169, !12168, !12167, !12166, !12165, !12164, !12163.

Status summary: 48 merged; 2 closed/unmerged (!12211 and !12180). Closed work was down-weighted relative to merged implementation. Master changes were weighted more heavily than release-branch cherry-picks, while substantive maintainer discussion was retained where useful.

| MR | Review weight | Notes |
|---|---|---|
| !12212 | Scanned | Merged GitLab CI cleanup collapsing multiline commands; infrastructure readability only. |
| !12211 | Discussion-focused (closed) | Display-filter proposal tried to reinterpret a syntactic protocol name as an unquoted value-string literal. João Valverde strongly rejected guessing alternate meaning for an otherwise well-defined expression and pointed to quoted value strings / the !12213 deprecation warning instead. Closed; useful parser-language design evidence, not an implementation exemplar. |
| !12210 | Scanned / later-corrected family | Merged display-filter change rendered default booleans as `True`/`False`. Peter Wu raised public-output/JSON compatibility concerns; later !12286 corrected native JSON boolean representation. Do not generalize the transient serialization behavior. |
| !12209 | Scanned | Merged GSM RLC/MAC correction for the 11-bit UL Packet Control Acknowledgement message-type length. Protocol-specific correctness fix. |
| !12208 | Discussion-focused | Merged boolean registration cleanup makes NULL `strings` use the documented True/False default and removes redundant `tfs_true_false` registrations. Later John Thacker discussion records an edge-case regression where custom/inverted TFS semantics interact with default parsing; retained as caution against conflating canonical booleans with field-specific labels. |
| !12207 | Scanned | Merged GSM CBCH `value_string` corrections. Reinforces value-table correctness but adds no new rule. |
| !12206 | Scanned | Merged RTP events correction for duplicate numeric ID 192 carrying conflicting meanings. Protocol table maintenance only. |
| !12205 | Scanned | Merged release-branch CI backport fixing the Windows package-upload tool path (`mc` ambiguity). No new convention beyond the master fix. |
| !12204 | Scanned | Merged release-branch CI backport of the same Minio `mc` path correction. |
| !12203 | Scanned | Merged master CI path correction selecting Minio `mc` rather than Visual Studio Message Compiler. Straightforward tool-resolution fix. |
| !12202 | Scanned | Merged BGP fix stores RFC 9072 extended optional-parameters length in 16 bits so offset advancement cannot wrap/truncate through an 8-bit temporary. Reinforces width matching for protocol length fields. |
| !12201 | Scanned | Merged release-branch backport using Minio `mc` for Windows package uploads. |
| !12200 | Scanned | Merged release-branch backport of Minio package upload. |
| !12199 | Scanned | Merged master switch to Minio `mc` for Windows package upload. CI infrastructure only. |
| !12198 | Deep | Guy Harris-authored/merged macOS setup fix forces CMake-built dependencies to record a usable full install name rather than `@rpath/<name>`, so Wireshark binaries run directly from the build directory on Sonoma/Xcode 15. Added durable build-environment/runtime-loader guidance. |
| !12197 | Scanned | Guy Harris-authored/merged macOS setup update to a current zlib release after the old download disappeared. Dependency version maintenance only. |
| !12196 | Scanned | Merged revert of temporary GitLab CI debugging. No durable engineering rule. |
| !12195 | Deep | Martin Mathieson-authored/merged typed-item checker follow-up makes `value_string` range validation mask-aware by using the number of bits represented by the field mask, and includes the mask in diagnostics. Promoted to field-value/checker semantics. |
| !12194 | Corroboration | Merged release backport adding HMAC-MD5 to the IS-IS authentication algorithm table; no new lesson beyond master behavior. |
| !12193 | Corroboration | Merged TACACS release backport removes `if (tree)` gating that suppressed expert info when dissecting with a NULL tree (for example the Expert Info dialog). Reinforces the existing rule that NULL tree must not suppress semantic processing/diagnostics. Original master change is !12110 and remains for a later batch. |
| !12192 | Corroboration | Merged GNW release backport replaces an assertion on an oversized packet-derived IntX value with expert information. Reinforces the existing malformed-input-vs-programmer-invariant distinction. |
| !12191 | Scanned | Merged PFCP `value_string` correction for values shifted by one byte. Field-table correctness only. |
| !12190 | Scanned | Merged temporary GitLab CI debugging; subsequently reverted by !12196. No durable rule. |
| !12189 | Scanned | Merged display-filter fix ensures FT_BOOLEAN fields with custom `true_false_string` labels can parse those field-specific symbolic values. Useful boolean/value-string edge case but later related changes continued evolving. |
| !12188 | Scanned | Merged Wi-SUN LBC-IE ID update from the assigned-value registry. Specification maintenance. |
| !12187 | Scanned | Merged MinGW portability fix avoids an MSVC-specific packing pragma under GCC. Reinforces compiler-specific guard discipline. |
| !12186 | Deep | John Thacker-authored/merged libwiretap merge redesign removes the practical all-files-open requirement. Guy Harris suggested raising the UNIX soft fd limit before batching; accepted code bounds the raise by platform limits, attempts it once, then batch-merges through temporary files while preserving ordering. Added `resource-limit-conventions.md`. |
| !12185 | Deep | Martin Mathieson-authored/merged typed-item checker begins validating registered `VALS` ranges against field width and explicitly notes the initial mask limitation, immediately completed by !12195. Promoted together with !12195. |
| !12184 | Scanned | Merged DHCPv6 Option 17 vendor-options custom dissector support. Extension-point implementation without new cross-cutting guidance. |
| !12183 | Scanned | Merged IEEE 1905 BSS Configuration Report true/false-string correction. Field presentation fix. |
| !12182 | Discussion-focused | Merged `tzset()` performance cleanup moves explicit setup out of every `localtime_r()`/`gmtime_r()` wrapper call. Guy Harris clarified that macOS notifies processes of timezone changes and updates conversions without repeated explicit calls when `TZ` is not set. High-authority platform detail retained in this ledger; too narrow for a new general convention. |
| !12181 | Scanned | Merged release-branch macOS code-signing update. Backport/infrastructure only. |
| !12180 | Scanned (closed) | Closed release-branch attempt at the same macOS code-signing update; superseded by merged adjacent backports, so down-weighted. |
| !12179 | Scanned | Merged release-branch macOS code-signing update. No new convention. |
| !12178 | Scanned | Merged ZBNCP spelling correction (`preambule` → `preamble`). |
| !12177 | Scanned | Merged master macOS code-signing update. Release infrastructure only. |
| !12176 | Deep | Guy Harris-authored/merged macOS setup fix detects generated `libffi.pc` metadata whose SDK include directory no longer exists after Xcode updates, removes it, and allows regeneration. Added stale-derived-build-metadata guidance. |
| !12175 | Deep | João Valverde-authored/merged CMake assertion redesign separates assertions from `WS_DEBUG`, follows conventional `NDEBUG` defaults, and documents assertion behavior for Debug/RelWithDebInfo versus Release/MinSizeRel. Added to CI/build-configuration conventions and connects directly to later !12230 Debug-test policy. |
| !12174 | Scanned | Merged IS-IS HMAC-MD5 support for RFC 5310-style authentication, justified against the RFC and real Nokia/Juniper behavior. Good spec/reference evidence but protocol-specific. |
| !12173 | Scanned | Merged new X.75 SLP/MLP dissector with example decoding and explicit dependency on the GSMTAP subtype update in !12172. New-protocol integration; no additional general rule promoted. |
| !12172 | Scanned | Merged GSMTAP support/dispatch for PPP, V.120, and other ISDN B-channel subtypes already having Wireshark dissectors. Straightforward dissector-table integration. |
| !12171 | Scanned | Merged Signal-PDU diagnostic wording cleanup so an empty configuration does not falsely imply dissection is disabled. User-facing diagnostic clarity only. |
| !12170 | Scanned | Merged `checkAPIs.py` improvement recognizes padding/width in forbidden `%ll` format patterns. Checker coverage maintenance. |
| !12169 | Scanned | John Thacker-authored/merged tshark Follow UTF-8 mode deliberately mirrors Qt's valid/invalid UTF-8 semantics while documenting remaining UI/CLI formatting differences. Corroborates existing shared text-conversion/interface-consistency guidance. |
| !12168 | Scanned | Merged release backport updating macOS code-signing identity. |
| !12167 | Scanned | Merged release backport updating macOS code-signing identity. |
| !12166 | Scanned | Merged master update to macOS code-signing identity. Infrastructure credential identity only. |
| !12165 | Deep | Guy Harris-authored/merged macOS setup workaround for GNU gettext's implementation-specific iconv configure test after macOS adopted a different POSIX-conforming iconv behavior. Added build-environment guidance distinguishing standards compliance from third-party configure assumptions. |
| !12164 | Scanned | Merged converter-tool extension for GLib integer format specifiers plus sample conversions. Migration tooling maintenance. |
| !12163 | Scanned | Merged Qt Lua-menu reload cleanup removes empty submenus after plugin actions move or disappear. UI/plugin lifecycle cleanup without broader rule. |

Notebook changes promoted from this batch:

- `field-value-semantics-conventions.md`: checker range validation for `VALS` must use a masked field's effective logical width; !12185 + !12195.
- `ci-test-configuration-conventions.md`: assertion enablement is distinct from `WS_DEBUG` and follows `NDEBUG`/build-type semantics; !12175, reinforcing later !12230.
- `resource-limit-conventions.md`: treat soft resource limits as recoverable when a semantics-preserving bounded algorithm exists; try a bounded unprivileged soft-limit raise, then batch with guaranteed termination; !12186 with Guy Harris review.
- `platform-build-environment-conventions.md`: revalidate SDK-derived metadata, account for implementation-specific dependency configure assumptions, and validate runtime loader metadata for build-tree execution; Guy Harris-authored !12165, !12176, and !12198.

Frontier check only (not reviewed): !12162 exists in the same corpus commit and is merged. It is therefore the next descending candidate unless a future corpus refresh introduces a higher-numbered previously unreviewed MR.