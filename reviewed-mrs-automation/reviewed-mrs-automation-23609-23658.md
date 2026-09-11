# Wireshark MR Review Automation: !23609–!23658

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Before selecting this batch, the already-reviewed set was rebuilt from the available tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md`, aggregate tracking, and the individual files under `reviewed-mrs-automation/`. Individual MR entries were used rather than treating a numeric range or range filename as proof of coverage. The separately represented higher-numbered material was retained, and the historical !17571–!17620 batch remains explicitly preserved/counts as reviewed. After intersecting that reviewed set with the corpus at the commit above, the fifty highest-numbered unreviewed corpus MRs were !23658 downward through !23609.

Exactly 50 previously unreviewed corpus MRs were reviewed, in descending order. The exact set is:

!23658, !23657, !23656, !23655, !23654, !23653, !23652, !23651, !23650, !23649,
!23648, !23647, !23646, !23645, !23644, !23643, !23642, !23641, !23640, !23639,
!23638, !23637, !23636, !23635, !23634, !23633, !23632, !23631, !23630, !23629,
!23628, !23627, !23626, !23625, !23624, !23623, !23622, !23621, !23620, !23619,
!23618, !23617, !23616, !23615, !23614, !23613, !23612, !23611, !23610, !23609.

## Review weighting and findings

- **!23658 — Scanned, merged release backport.** Updates libssh to 0.11.4 for a supported branch; dependency maintenance only.
- **!23657 — Scanned, merged release backport.** BLF tracks the final object and pads it when closing a log container so object/container alignment remains valid. Format-specific writer fix; no new cross-project rule.
- **!23656 — Deep/promoted, merged master; authored and merged by Michael Mann.** Removes copied `VALS_EXT_PTR()` uses from static `value_string_ext` registrations and reserves the macro for pointers that are actually dynamic at runtime. Promoted to `field-registration-conventions.md`.
- **!23655 — Scanned, merged.** O-RAN FH CUS tap carries the eAxC subfields separately so consumers can display their semantic components correctly. Tap/protocol-specific improvement.
- **!23654 — Deep/corroboration, merged.** Continues the migration from legacy `tvb_find_line_end()` patterns to remaining-length/status-aware APIs and unsigned offsets. Reinforces existing parser/API-domain guidance.
- **!23653 — Deep, merged master with extensive discussion and sample captures.** Moves thousands of QCDIAG log codes to XML resources and delays field registration/loading work to avoid startup impact. Review covered libxml2 use, parse-error handling, validation aids, packaging, sample captures, and leak/corruption checks. Valuable architecture evidence, but no additional notebook rule was promoted beyond already-established external-data/testing/error-handling guidance.
- **!23652 — Scanned, merged documentation.** Updates Developer's Guide text around `WIRESHARK_BASE_DIR` and Homebrew. Documentation/platform maintenance.
- **!23651 — Deep/corroboration, merged.** More remaining-length line-search/unsigned-offset migration and removal of excessive `tvb_ensure_bytes_exists()` use. Reinforces existing TVB parsing guidance.
- **!23650 — Scanned, merged.** Coloring Rules UI can expose multiple matching colors. UI feature; no general coding rule extracted.
- **!23649 — Scanned, merged Coverity follow-up.** Corrects a typo introduced while eliminating a double fetch in DSR. Reinforces careful mechanical API migrations but adds no new rule.
- **!23648 — Scanned, merged.** Introduces the first PN-SXP dissector implementation. Protocol feature with standards-based decoding; no durable cross-project correction extracted.
- **!23647 — Scanned, merged Coverity fix.** Corrects argument order for `tvb_find_line_end_remaining()` in Diameter 3GPP. Reinforces exact API-contract review during migrations.
- **!23646 — Deep/promoted, merged master; authored by John Thacker and accepted by Gerald Combs.** OSS-Fuzz found stack-use-after-return after a GUID lookup changed from a container that copied keys to `wmem_map`, which retains the supplied key pointer. The accepted fix copies the GUID to `wmem_epan_scope()` before insertion. Promoted to `allocator-scope-conventions.md` as a container-migration ownership rule.
- **!23645 — Down-weighted, closed/unmerged.** Proposed deprecation/commenting-out of unsigned `tvb_find...` functions. Michael Mann questioned retaining commented-out code rather than deleting it because source control preserves history. Since the MR was closed and the API direction continued elsewhere, it was not promoted as accepted project policy.
- **!23644 — Scanned, merged.** Removes unnecessary protocol-name macros from another large non-ASN.1 dissector batch. Cleanup toward direct, readable registration data; no separate rule needed.
- **!23643 — Scanned, merged.** Removes protocol-name macros from ASN.1-based dissectors. Same cleanup direction as !23644.
- **!23642 — Scanned, merged.** Extends IEEE 802.11 RSNXE parsing and adds EPPKE support from the current specification draft. Standards-tracking protocol work.
- **!23641 — Scanned, merged.** Corrects source-file names in dissector header comments. Mechanical source hygiene.
- **!23640 — Scanned/corroboration, merged.** Supported-branch form of the tshark exit-code cleanup family; reinforces semantic process-status handling.
- **!23639 — Deep/promoted, merged master; authored and merged by Guy Harris.** Replaces misleading reused tshark exit status 2 with failure-specific `WS_EXIT_*` values and moves `WS_EXIT_NOW` outside the valid UNIX exit-status domain because it is an internal “already printed; terminate successfully” sentinel, not an OS status. Promoted to `cli-exit-status-conventions.md` with extremely high weight.
- **!23638 — Scanned, merged.** Welcome page UI overhaul with InfoBanner carousel. UI feature, not a reusable core convention.
- **!23637 — Scanned, merged.** GSM SIM adds ISD-R proprietary application-template fields. Protocol-specific standards support.
- **!23636 — Deep/corroboration, merged.** Another remaining-length/unsigned-offset parser migration. Reinforces existing parser-control and type-domain rules.
- **!23635 — Deep/corroboration, merged master; review requested from Guy Harris and John Thacker.** Centralizes file-path comparison and queries filesystem case sensitivity at runtime on platforms where it is not a fixed OS-wide property. Strong portability evidence against assuming “macOS/FreeBSD means case-insensitive,” but the principle is already represented by platform/runtime-semantics guidance, so no duplicate rule was added.
- **!23634 — Scanned, merged.** Improves NFSv4 dissection and filename snooping. Protocol-specific correctness work.
- **!23633 — Scanned/corroboration, merged backport.** Invalid read filters use `WS_EXIT_INVALID_FILTER` rather than the invalid-interface status. Same semantic-status family as !23631/!23639.
- **!23632 — Scanned/corroboration, merged backport.** Same invalid-filter exit-status correction for another supported branch.
- **!23631 — Deep/corroboration, merged master.** tshark/strato report invalid read filters with a status whose semantic meaning matches the failure rather than reusing the invalid-interface code. Prefigures and corroborates Guy Harris's broader !23639 rule.
- **!23630 — Scanned, merged.** Consolidates duplicated workspace-state filename comparison into a common routine. Straightforward helper reuse, reinforced by !23635's later portability refinement.
- **!23629 — Deep/corroboration, merged.** Continues `tvb_find_line_end()` replacement and unsigned-offset conversion. Existing parser API/type-domain guidance already covers it.
- **!23628 — Deep/promoted, merged master; authored by John Thacker.** Sanitizes newlines in user-provided values before writing line-oriented preferences, recent-state, and filter files so one logical value cannot synthesize additional records when read back. Promoted to `configuration-serialization-conventions.md`. A post-merge report that `G_REGEX_MATCH_DEFAULT` was too new for Ubuntu 22.04 was retained as an implementation-compatibility caution.
- **!23627 — Scanned, merged.** Updates the File → Open Recent menu when recent-file availability changes. UI state synchronization only.
- **!23626 — Scanned, merged.** Restores a comment noting that filename matching can be case-insensitive on macOS. This concern evolves into the runtime filesystem test in !23635.
- **!23625 — Deep/corroboration, merged master.** Silabs DCH catches nonfatal exceptions thrown by nested IEEE 802.15.4 dissection and renders the exception against the sub-tvb instead of allowing malformed nested data to abort the wrapper's useful output. Reinforces existing exception-boundary/malformed-input handling; no duplicate rule added.
- **!23624 — Scanned, merged.** Spelling fixes only.
- **!23623 — Deep/corroboration, merged.** Replaces legacy line-end searching and converts offsets to unsigned types. Reinforces existing parser/API-domain guidance.
- **!23622 — Scanned, merged.** Welcome-page spacing cleanup. UI-only.
- **!23621 — Scanned, merged static-analysis fix.** Removes a Megaco dead store reported by Clang Analyzer. Reinforces existing static-analysis cleanup guidance.
- **!23620 — Deep/corroboration, merged.** Telnet adopts `tvb_find_uint8_length()` and unsigned offsets. Same status/out-parameter and offset-domain direction already recorded elsewhere.
- **!23619 — Scanned, merged.** QCDIAG refactors per-packet data and addresses field issues leading into later dictionary work. Protocol architecture cleanup but no distinct general rule extracted.
- **!23618 — Scanned, merged.** Removes a redundant vertical layout from Capture File Properties. UI cleanup.
- **!23617 — Deep/corroboration, merged.** Multiple dissectors move to length/remaining-returning TVB search helpers. Reinforces existing parser-search and unsigned-offset guidance.
- **!23616 — Deep, merged master with maintainer discussion.** Adds structured TCP RST diagnostic-payload dissection; Jaap Keuter requested updating to the then-current draft before merge and the submission did so. Useful evidence to implement current protocol drafts/spec revisions and expose binary structure as filterable fields rather than misleading raw text, but no new notebook rule was necessary.
- **!23615 — Scanned, merged.** CAPWAP adds the RFC 5415 WTP Static IP Address Information element. Standards-specific feature.
- **!23614 — Scanned, merged automatic update.** Generated registries/translations/data refresh; no durable convention.
- **!23613 — Scanned, merged automatic update.** Generated registries/translations/data refresh; no durable convention.
- **!23612 — Scanned, merged automatic update.** Generated registries/translations/data refresh; no durable convention.
- **!23611 — Scanned, merged Coverity fix.** Removes unused/dead values in tshark and corrects a nearby comment. Existing static-analysis guidance covers it.
- **!23610 — Deep, merged.** Improves tshark profile selection, including read-only use of global profiles and clearer profile-directory behavior. Configuration behavior/API work, but no additional durable rule was extracted beyond existing profile/configuration semantics.
- **!23609 — Deep/promoted, merged master with substantial maintainer review.** The initial patch tried to distinguish PortableApps with a compile-time `PORTABLE` macro. John Thacker explained that PortableApps repackages the same compiled executable and supplies its distinction at launch time; the final accepted code detects `PORTABLEAPPS.com` at runtime so only that package form disables installer-style update checks. Guy Harris participated in the revision history. Promoted to `packaging-runtime-conventions.md`.

## Durable notebook promotion

- `allocator-scope-conventions.md`: container/API migrations must re-audit whether keys and values are copied or borrowed; a new container retaining a pointer requires lifetime-safe storage rather than stack-backed keys (!23646, John Thacker, OSS-Fuzz, merged master).
- `cli-exit-status-conventions.md`: external process exit statuses should identify the real failure class, while internal command-line control sentinels must remain outside the valid OS exit-status domain (!23639, authored and merged by Guy Harris; corroborated by !23631–!23633/!23640).
- `configuration-serialization-conventions.md`: sanitize or encode record separators in user-controlled values before writing line-oriented configuration/state files (!23628, John Thacker, merged master).
- `field-registration-conventions.md`: use `VALS_EXT_PTR()` only when the `value_string_ext` pointer itself is dynamic at runtime; static extensions should be registered directly (!23656, authored and merged by Michael Mann).
- `packaging-runtime-conventions.md`: packaging-specific behavior must be keyed from the layer where the distinction actually exists; when several packages ship the same binary, use authoritative runtime/package signals rather than a nonexistent compile-time macro (!23609, direct John Thacker correction with further maintainer participation).

Merged master work and accepted maintainer reasoning were weighted most heavily. In particular, Guy Harris's authored/merged !23639 was given the strongest weight; John Thacker's merged memory-safety and serialization work in !23646/!23628 and his direct correction in !23609 were also treated as high-confidence evidence. Closed !23645 was explicitly down-weighted. Repeated parser migrations, backports, generated updates, dependency updates, protocol-only additions, and mechanical cleanup were counted in the exact reviewed set without creating duplicate notebook rules.