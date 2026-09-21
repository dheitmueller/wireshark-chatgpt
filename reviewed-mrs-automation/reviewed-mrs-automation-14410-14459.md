# Automated MR review ledger: !14410-!14459

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `1c7ad509887ee25079a7865cc62ba18cba06f49a`
Notebook starting commit: `9e95e6621d6c3d8d3ef333b2923d0276b13899db`
Review direction: descending from the highest-numbered previously unreviewed MR.

## Selection and deduplication

Before selecting this batch, the reviewed set was reconciled against `reviewed-mrs.md`, the available per-run files under `reviewed-mrs-automation/`, and the immediately preceding exact ledger `reviewed-mrs-automation-14460-14509.md`. The notebook and corpus heads had not changed since that preceding ledger's full reviewed-set reconciliation, so that exact membership snapshot remained current. The historical !17571-!17620 batch remains explicitly included. Later high-numbered ledgers are also part of the reviewed set, including the mixed !26567-!26577 / !14710-!14748 run and the separate !26566 run. Selection was performed by exact MR-number membership, not by assuming any numeric range was complete.

At corpus commit `1c7ad509887ee25079a7865cc62ba18cba06f49a`, the fifty highest-numbered MRs not already in that reviewed set are exactly !14459 through !14410. This run reviews all fifty and no others.

Outcome: 46 merged MRs and four closed/unmerged MRs (!14458, !14446, !14427, and !14426). Closed/unmerged work is down-weighted, especially where merged successors or later accepted architecture provide stronger precedent.

## Exact MRs reviewed

| MR | State | Review | Notes |
|---|---|---|---|
| !14459 | merged | Scanned | Couchbase status/feature/system-event updates. Alexis La Goutte questioned event 2; the author explained the never-shipped FlushCollection value and aligned the table with the documented reserved/unused value. Protocol-local completeness, no new general rule. |
| !14458 | closed/unmerged | Down-weighted | Earlier/duplicate Couchbase status update superseded by merged !14459. Not treated as implementation precedent. |
| !14457 | merged | Deep | John Thacker-authored preference migration for hidden columns reads both old format-based and new index-based representations, gives the new form precedence, and continues writing the old form for cross-version compatibility. Promoted to `preference-architecture-conventions.md`. |
| !14456 | merged | Scanned | Protobuf fallback/last-ditch message-name preference. Useful protocol configuration feature; no distinct cross-cutting rule beyond existing preference architecture. |
| !14455 | merged | Deep | John Thacker reworks extcap configuration to expose semantically distinct Save, Start, and Discard actions and preserve Save-on-Start policy. Combined with !14423 and promoted to `ui-preference-editing-conventions.md`. |
| !14454 | merged | Corroborating | release-4.2 backport of !14423's extcap preference-write fix. Confirms the persistence bug was important enough for stable propagation. |
| !14453 | merged | Scanned | X.509 organizationIdentifier dissection update. Protocol-local standards coverage; no new general convention. |
| !14452 | merged | Deep | Telnet Info-column summaries. John Thacker review explicitly distinguishes borrowed static `col_set_str()` text from copying `col_add_str()` for dynamic strings and favors a preexisting fallback so exceptions do not leave a blank Info column. Promoted to `column-string-conventions.md`. |
| !14451 | merged | Historical/corroborating | PLDM version-string rework. Later review exposed remaining-capacity handling that was fixed by already-reviewed !14478, so this MR is useful intermediate buffer-safety evidence but not the final exemplar. |
| !14450 | merged | Deep/corroborating | John Thacker prevents `matches` from treating `FT_FRAMENUM`'s overloaded `hfinfo->strings` as a value-string table, avoiding a crash. Reinforces existing type-aware display-metadata guidance; later reviewed discussion supplies broader architectural treatment. |
| !14449 | merged | Scanned | Adds Copy as C Array. GUI feature with no additional durable architecture lesson. |
| !14448 | merged | Corroborating | release-4.2 backport removing an invalid Homebrew Lua 5.1 assumption. Reinforces existing supported-dependency/version guidance. |
| !14447 | merged | Corroborating | Master version of the Homebrew Lua assumption fix; existing dependency compatibility guidance already covers it. |
| !14446 | closed/unmerged | Down-weighted | Earlier Copy as C Array proposal superseded by merged !14449. |
| !14445 | merged | Corroborating | PCI ID lookup switches to standard `bsearch` while regenerated data stays synchronized. Reinforces generated-source-of-truth and standard-library reuse guidance. |
| !14444 | merged | Scanned | Automated data/update maintenance; no substantive review lesson. |
| !14443 | merged | Scanned | Stable-branch automated data/update maintenance; no new convention. |
| !14442 | merged | Scanned | Stable-branch automated data/update maintenance; no new convention. |
| !14441 | merged | Scanned | Stable-branch automated data/update maintenance; no new convention. |
| !14440 | merged | Scanned | Thrift field-id proto item must exist for sub-dissector/expert-reporting needs even when ordinary tree display is not requested. Useful protocol implementation detail, but too narrow for a new notebook rule. |
| !14439 | merged | Scanned | Spelling/wording cleanup only. |
| !14438 | merged | Deep/corroborating | COSE map lookahead. John Thacker catches heap data leaked when nested dissection throws and a helper that may return NULL before `tvb_utf_8_isprint()` use; both were fixed. Strong corroboration for existing allocator-scope, exception-boundary, and helper-postcondition rules. |
| !14437 | merged | Scanned | Documentation of the then-current Lua-version support floor. Historical dependency documentation, no new rule. |
| !14436 | merged | Deep/corroborating | Extcap initialization/persistence lifecycle fix. Review exposes legacy extcap-path directories/FIFOs and the need to write main preferences independently; later merged !14470/!14471 are the stronger final precedents already promoted to configuration-file I/O guidance. |
| !14435 | merged | Scanned | release-4.2 VP9 scalability-count fix; stable backport of protocol correctness work. |
| !14434 | merged | Scanned | IPsec ESP ICV verification with Extended Sequence Numbers and compatible UAT support. Protocol feature; no additional broad convention. |
| !14433 | merged | Corroborating | Martin Mathieson converts PLDM parsing to `proto_tree_add_item_ret_uint()` instead of separately fetching the same wire value, while review also checks an offset-sensitive field. Reinforces existing typed-item/helper and single-source parsing guidance. |
| !14432 | merged | Scanned | macOS application plist metadata cleanup. Guy Harris caught a mistaken reference type/number, but the implementation itself is packaging metadata, not a reusable coding rule. |
| !14431 | merged | Scanned | Protobuf `application/x-protobuf` handling; follow-up need for an unknown-message-name preference is addressed by !14456. |
| !14430 | merged | Scanned | VP9 spatial-layer display-field support; protocol presentation feature. |
| !14429 | merged | Corroborating | GTPv2 conversion to more `proto_tree_add_bitmask_list()` usage. Reinforces use of framework helpers for structured bitfield display. |
| !14428 | merged | Discussion-focused | LLDP additional fields and fractional-calculation fixes. Guy Harris corrected the MR title; Alexis La Goutte requested keeping an ad-hoc pcap out of `test_captures` and using the issue attachment instead. Useful submission-hygiene evidence, but no new broad rule. |
| !14427 | closed/unmerged | Down-weighted | Draft VP9 spatial-layer work superseded by merged !14430 and related stable fix !14435. |
| !14426 | closed/unmerged | Deep negative evidence | SOME/IP attempted UAT-registration reordering. Lars Völker reproduced the problem but rejected reordering as a general solution because configuration types can have cyclic dependencies, and asked for an issue instead. Later merged !14779/!14793 provide the accepted lifecycle/invalidation architecture already recorded in `state-refresh-conventions.md`. |
| !14425 | merged | Scanned | PLDM formatting/readability cleanup intentionally avoiding behavior change. Review discusses local-versus-project-wide formatting consistency; no new durable rule. |
| !14424 | merged | Deep | VMware vSPC vMotion state spans two Telnet conversations and must work in one pass. Adds blob conversation identity and carries sequence-length state across the protocol-level handoff. Promoted to `single-pass-analysis-conventions.md`. |
| !14423 | merged | Deep | John Thacker fixes extcap persistence by keeping tentative dialog edits out of the real preference pointers until the store path commits them; premature mutation defeated change detection. Combined with !14455 and promoted to `ui-preference-editing-conventions.md`. |
| !14422 | merged | Scanned | Extcap example-script cleanup for Python 3.12 SyntaxWarnings. Tooling/documentation maintenance. |
| !14421 | merged | Historical | John Thacker explicitly states command-line preference overrides must be either reapplied or dropped on profile changes; this MR chose dropping them to avoid latent reapplication on Lua reload. Later merged !16250 adopted reapplication as the current policy, so !14421 is retained as lifecycle evidence, not current behavioral precedent. |
| !14420 | merged | Scanned | Coverity-driven copy/paste correctness fix. Static-analysis maintenance, no separate rule. |
| !14419 | merged | Scanned | Extcap callback documentation updated for parallel/threaded operation and obsolete return-value semantics. API documentation maintenance. |
| !14418 | merged | Scanned | Preference callback cleanup allowing NULL/free-only handling instead of manufacturing a temporary list. Ownership/efficiency cleanup covered by existing guidance. |
| !14417 | merged | Deep | macOS Show in Finder moves from spawning `osascript`/AppleScript with manual escaping to direct Cocoa `NSWorkspace` API use. Guy Harris approved and handled the merge. Promoted to `platform-native-integration-conventions.md`. |
| !14416 | merged | Scanned | release-3.6 CI/version backport maintenance. |
| !14415 | merged | Scanned | release-4.0 `make-version` argparse fix/backport. Build/release tooling maintenance. |
| !14414 | merged | Scanned | release-4.0 CI/version maintenance. |
| !14413 | merged | Scanned | release-3.6 release-note artifact update. |
| !14412 | merged | Scanned | release-4.0 release-note artifact update. |
| !14411 | merged | Scanned | release-4.2 release-note artifact update. |
| !14410 | merged | Scanned | release-3.6 version bump/release preparation. |

## Durable notebook changes promoted from this batch

- `preference-architecture-conventions.md`: persisted preference migrations need an explicit compatibility window, read precedence, and deliberate old/new write policy. Primary evidence: merged !14457, authored and merged by John Thacker.
- `single-pass-analysis-conventions.md`: when a logical protocol exchange spans lower-layer conversations, one-pass correctness may require a higher-level conversation identity that carries state across the handoff. Primary evidence: merged !14424.
- `ui-preference-editing-conventions.md` (new): keep tentative dialog values separate from canonical preference backing storage until an explicit commit; represent Save/Start/Discard as distinct semantic actions. Primary evidence: merged !14423 and !14455, both authored by John Thacker and merged by Anders Broman.
- `column-string-conventions.md` (new): use column string APIs according to string lifetime/copy semantics and establish a safe fallback before exception-prone parsing. Primary evidence: merged !14452 with direct John Thacker review and approval.
- `platform-native-integration-conventions.md` (new): prefer direct native platform APIs over launching a scripting intermediary when the OS SDK already exposes the operation. Primary evidence: merged !14417 with Guy Harris approval/merge involvement.

## Strong corroborating and historical evidence retained without duplicate notebook rules

- !14438 independently reinforces exception-safe Wireshark-scoped allocation and checking helper return contracts before TVB use.
- !14450 reinforces that `hfinfo` auxiliary fields are type-dependent metadata and cannot be interpreted uniformly merely because a pointer is non-NULL.
- !14436 is useful historical extcap configuration evidence, but the already-reviewed !14470/!14471 provide the cleaner accepted object-type and failure-isolation rules.
- !14426 is intentionally negative evidence: reordering interdependent SOME/IP UATs was rejected because cyclic dependencies make order-based initialization inherently fragile. Later !14779/!14793 provide stronger accepted lifecycle behavior.
- !14421 captures the invariant that command-line overrides need an explicit profile-switch lifecycle. Its chosen policy was later superseded by !16250, which re-applies active higher-precedence command-line overrides and is the current stronger precedent.
- !14451 is intermediate PLDM buffer work; already-reviewed !14478 is the stronger final remaining-capacity fix.

## Frontier after this run

`mr_14409.json` exists in the same corpus commit and is merged. Therefore the corpus is not exhausted. If no newer previously unreviewed MR is added before the next run, !14409 is the next descending candidate.
