# Wireshark MR review batch !23009–!23058

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed set from `reviewed-mrs.md`, the aggregate automation ledger, and the available per-run files under `reviewed-mrs-automation/`, treating individual MR numbers as authoritative rather than assuming numeric ranges were complete. The previously reviewed historical batch !17571–!17620 remains explicitly preserved and counted. From the corpus commit above, the fifty highest-numbered entries not in that reviewed set were exactly the MRs listed below. No MR below !23009 is counted in this run.

Review weighting: merged master changes and explicit maintainer decisions were weighted most heavily; merged stable-branch duplicates were treated mainly as corroboration; closed/superseded JSONPRO attempts and the still-draft !23013 implementation were down-weighted. Guy Harris's technical comments in !23013 were treated as highly authoritative, but only the portion independently realized by merged !23020 was promoted as settled guidance.

| MR | Review | Finding |
|---|---|---|
| !23058 | Discussion/diff reviewed, merged | SMB TFS restoration after prior edits; post-merge discussion warns that conflict-resolution strategy can silently overwrite earlier semantic changes. Useful caution, but no separate notebook rule promoted from this instance alone. |
| !23057 | Deep, merged | TCP out-of-order-reassembly regression. Reviewer requested a concrete reproducer when existing captures did not show the failure; contributor supplied an anonymized PCAP with packet/state progression and older-version comparison. Strong corroboration of capture-backed regression review and precise commit-subject guidance. |
| !23056 | Deep, merged | John Thacker removes automatic lwres UDP/921 binding because the port was never IANA assigned, the protocol is obsolete, and the dissector lacked other discrimination. Promoted to `dissector-registration-conventions.md`. |
| !23055 | Deep, merged | John Thacker tightens NLM request/reply handling: request the actual required `tvb_get_ptr` length, use generated FT_FRAMENUM links, and avoid unsafe early cookie access. Reinforces existing TVBuff bounds and state/linkage guidance. |
| !23054 | Scanned, merged | Gerald Combs updates macOS Tahoe/Liquid Glass icon assets and removes obsolete conditional/resource paths. Platform packaging/resource maintenance; no reusable rule promoted. |
| !23053 | Deep, merged | Dictionary-driven JSON tree integration after a long first-contribution review. Michael Mann explicitly required revising the existing MR rather than serial replacement MRs so outstanding review context remains coherent. Promoted to `submission-conventions.md`. |
| !23052 | Scanned, merged | IDP packet-type/well-known-socket decoding update based on registered values. Protocol-specific enhancement; no distinct general rule. |
| !23051 | Scanned, merged | DocBook archive URL maintenance on a maintained branch. Backport/packaging corroboration only. |
| !23050 | Scanned, merged | DocBook archive URL maintenance. Routine dependency/download-location fix; no new convention. |
| !23049 | Deep, merged | John Thacker documents that wiretap writers using `wtap_dump_file_seek()` must declare `writing_must_seek`; seeking compressed output is unsupported. Promoted with !23045/!23047 to `wiretap-writer-conventions.md`. |
| !23048 | Scanned, merged | BLF seek-required stable-branch counterpart. Corroborates the master writer-capability fix. |
| !23047 | Deep, merged | BLF checks `wtap_dump_file_tell()` despite the higher-level seek capability invariant. Reinforces existing defensive writer-error checking and the new seek-capability rule. |
| !23046 | Scanned, merged | Wiretap open-failure cleanup plugs ownership leaks in partially constructed dump structures. Reinforces cleanup-on-failure/lifetime discipline. |
| !23045 | Deep, merged | BLF declares `writing_must_seek` so incompatible/compressed output fails before writing rather than at a later seek. Promoted to `wiretap-writer-conventions.md`. |
| !23044 | Scanned, merged | ptvcursor converts non-string get-function lengths to unsigned domains. Reinforces existing unsigned offset/length API guidance. |
| !23043 | Scanned, merged | MySQL binary-field flag decoding. Protocol-specific field semantics; no general rule promoted. |
| !23042 | Scanned, merged | BLF dump-path lifetime cleanup. Reinforces writer cleanup and failure-path ownership guidance. |
| !23041 | Discussion/diff reviewed, merged | Splits a Stratoshark-specific status bar from a shared base so base code no longer asks which application it is running in. Reinforces existing frontend specialization/avoid-runtime-product-branching guidance. |
| !23040 | Scanned, merged | Adds IKETCP (RFC 9329) support to ISAKMP. Protocol feature; no independent project convention. |
| !23039 | Deep, merged stable maintenance | BLF buffer-overrun fix/backport: in-memory C struct size varies with alignment while the serialized header length is fixed. Strong writer-layout safety evidence; retained as corroboration rather than adding a redundant broad serialization rule. |
| !23038 | Scanned, merged | SA1P Ubuntu-builder compile fix. Portability/build maintenance only. |
| !23037 | Deep, merged | Master BLF buffer-overrun fix: allocate the actual 144 serialized bytes rather than assuming `sizeof(blf_fileheader_t)` supplies them across 32/64-bit alignment; ASAN reproducer described. Strong serialization/layout safety evidence. |
| !23036 | Scanned, merged | Corrects `tvb_find_line_end_unquoted` regression. Local TVBuff helper fix; existing parser/helper testing guidance applies. |
| !23035 | Discussion/diff reviewed, merged | Creates Stratoshark-specific PlotDialog specialization rather than conditional application checks in the shared base. Reinforces existing frontend architecture. |
| !23034 | Scanned, merged | Adds unsigned-offset/length `tvb_get_token_len_remaining`. Part of the established unsigned TVBuff API migration. |
| !23033 | Scanned, merged | LLDP loop condition rewritten to avoid arithmetic overflow. Reinforces arithmetic-before-bounds-check guidance. |
| !23032 | Scanned, merged | BGP multicast NLRI underflow guard. Reinforces existing checked arithmetic/bounds guidance. |
| !23031 | Scanned, merged | PROFINET rejects/handles total lengths shorter than the header rather than overflowing subtraction. Reinforces length-underflow validation. |
| !23030 | Scanned, merged | TECMP removes redundant `MAX(0, ...)` after remaining-length APIs became unsigned. API-domain cleanup; no new rule. |
| !23029 | Scanned, merged | DCT2000 replaces repeated byte fetch/`snprintf` construction with TVBuff/string-buffer helpers. Reinforces helper reuse and fetch-once guidance. |
| !23028 | Scanned, merged | Validates initial offset in a TVBuff helper after static-analysis finding. Reinforces offset-domain validation. |
| !23027 | Scanned, merged | Adds unsigned `tvb_find_line_end_unquoted_remaining`. Existing TVBuff API-domain guidance. |
| !23026 | Scanned, merged | Converts expert helpers to remaining-length APIs. Existing API migration guidance. |
| !23025 | Scanned, merged stable maintenance | Happy New Year 2026 release-branch cherry-pick. Administrative/version text update only. |
| !23024 | Scanned, merged stable maintenance | Happy New Year 2026 release-branch cherry-pick. No durable engineering convention. |
| !23023 | Scanned, merged | Happy New Year 2026 update. No durable engineering convention. |
| !23022 | Scanned, merged | Adds unsigned remaining-length line-end API variants. Existing offset/length domain guidance. |
| !23021 | Scanned, merged | WSLua gcrypt conversions avoid routing 64-bit `lua_Integer`/`size_t` values through 32-bit intermediates. Reinforces width-preserving conversion guidance. |
| !23020 | Deep, merged | John Thacker uses `pcap_init()` on Windows when available to request UTF-8 diagnostics and disable legacy device-string encoding behavior. Together with Guy Harris's rationale in !23013, promoted to `capture-platform-conventions.md`. |
| !23019 | Deep, merged | Michael Mann moves UI-plugin registration out of epan into a UI-facing library with minimal dependencies and updates plugin registration/demo infrastructure. Strong frontend/layering evidence; retained as architecture corroboration rather than duplicating existing frontend separation rules. |
| !23018 | Deep, merged | Martin Mathieson introduces static checking for duplicate TVBuff fetches and fixes representative findings; discussion notes hundreds of candidates and type-width tradeoffs. Reinforces fetch-once guidance and the need to validate checker-driven rewrites semantically. |
| !23017 | Scanned, merged | GSM SIM field abbreviation correction. Local field-registration maintenance. |
| !23016 | Scanned, merged | Removes never-implemented codec version API left from a template. API cleanup; no new convention. |
| !23015 | Discussion-focused, closed/superseded | Earlier JSONPRO plugin submission. Down-weighted as superseded by the reworked merged !23053; useful mainly as review-history context for the same-MR amendment rule. |
| !23014 | Scanned, merged | Additional expert-helper remaining-length conversion. Existing TVBuff API guidance. |
| !23013 | Deep discussion, draft/open | John Thacker Linux/libpcap timeout proposal remained unmerged. Guy Harris supplied highly authoritative details on Linux timeout semantics and on `pcap_init()`/UTF-8 compatibility. Linux design is not promoted; Windows encoding rationale is independently validated by merged !23020. |
| !23012 | Scanned, merged | RTPS boolean test correction (`OR` vs `AND`) after prior flag-state assumptions. Local logic bug; no new broad rule. |
| !23011 | Scanned, merged | Uses `ws_error` for additional `ENABLE_CHECK_FILTER` failures. Diagnostic consistency cleanup. |
| !23010 | Scanned, merged | Corrects G729 null-pointer check to test the proper structure; Coverity-driven local fix. Reinforces checking the actual object whose dereference follows. |
| !23009 | Scanned, merged | Removes unnecessary `cfile.h` inclusion from public `packet-ipsec.h`, reducing public-header dependencies. Reinforces existing header-dependency minimization guidance. |

## Promoted durable guidance

- `submission-conventions.md`: amend the existing MR when responding to review so unresolved discussion remains coherent; replacement MRs for the same logical change should not discard review history.
- `dissector-registration-conventions.md`: do not automatically claim obsolete/unregistered ports without authoritative assignment or another reliable discriminator.
- `capture-platform-conventions.md`: on Windows, use optional libpcap initialization to request UTF-8 diagnostics when available; preserve runtime compatibility rather than assuming the new API exists everywhere.
- `wiretap-writer-conventions.md`: writers that require random access must declare `writing_must_seek` so unsupported destinations fail before output starts, while individual seek/tell failures remain checked.

Exactly 50 MRs are counted in this run: !23009, !23010, !23011, !23012, !23013, !23014, !23015, !23016, !23017, !23018, !23019, !23020, !23021, !23022, !23023, !23024, !23025, !23026, !23027, !23028, !23029, !23030, !23031, !23032, !23033, !23034, !23035, !23036, !23037, !23038, !23039, !23040, !23041, !23042, !23043, !23044, !23045, !23046, !23047, !23048, !23049, !23050, !23051, !23052, !23053, !23054, !23055, !23056, !23057, !23058.