# Automated Wireshark MR review — !9561 through !9512

Review date: 2026-10-31
Corpus: `dheitmueller/wireshark-corpus-mrs`
**Corpus commit:** `ddcaa22b51c68f594e425a23388c3a2086813054`
Notebook starting commit: `8e2aa79b34792a0ddf6eafe64b68b401a47f3885`
Direction: numerically descending, with exact reviewed-membership checking.

Exactly **50 distinct previously unreviewed MR records** were examined: **49 merged** and **one closed without merge (!9526)**. All 50 files `mr_9561.json` through `mr_9512.json` exist in this corpus. The next expected candidate is !9511, subject to a new inventory.

## Selection safeguards and evidence

Consulted `reviewed-mrs.md`, supplemental `reviewed-mrs-automation/reviewed-mrs-automation.md`, the repository's complete per-run/exceptional ledger inventory, the immediately preceding explicit ledger (`reviewed-mrs-automation-9562-9612-skip-9604.md`), and exceptional/noncontiguous entries. The immediately preceding ledger states that it checked the then 354 per-run/exceptional files and established that all higher-numbered corpus records were accounted for. This run independently checked that the normal ledger files start no lower than !9613, inspected the supplemental and aggregate entries, examined the exceptional entries for lower-numbered membership, and did not infer a reviewed set from a numeric filename interval. **The old !17571–!17620 ledger was fetched and independently counted at 50 distinct MR rows**, with no modification.

Each selected JSON's MR metadata, merged/closed state, non-system discussions, available changed-file names and diff excerpts was inspected; high-value items had their relevant complete diff hunks and substantive human comments examined in depth. The archive provides patches, not a live tested build. No compilation, GUI regression test, dissector suite or fuzzing was performed in this run. Merged corrections and authoritative reviewer findings outrank abandoned proposals and already-reverted merges.

## Exact MRs reviewed

| MR | Outcome | Review depth | Evidence and finding |
|---|---|---|---|
| !9561 | merged | Scanned | AUTHORS entry format: removes a nonstandard dual-address entry. |
| !9560 | merged | Scanned | Qt About-dialog wording/capitalization cleanup. |
| !9559 | merged | Discussion-focused | Moves personal extcap path; Gerald requested release notes. Runtime discovery corrected later in !9566. |
| !9558 | merged | Scanned | NSIS uninstall/legacy-entry maintenance. |
| !9557 | merged | Discussion-focused | RTPS instance-state messages; Alexis raised trailing whitespace and static-analyzer null dereferences, Gerald referenced !9681 macOS fix. |
| !9556 | merged | Scanned | GTPv2 tracks MODIFY_BEARER_REQUEST for inter-RAT mobility sessions. |
| !9555 | merged | Scanned | AUTHORS generator updates the canonical file in place. |
| !9554 | merged | Deep | USB SET_ADDRESS resets old endpoint reassembly state; default address 0 is special. Discussion references !9538 regression. |
| !9553 | merged | Scanned | Adds documentation path/URL accessors with documented allocated return ownership. |
| !9552 | merged | Scanned | Martin Mathieson fixes cross-dissector typed-item registration mismatches found by checker. |
| !9551 | merged | Scanned | H.261 vmvd encoding fix on maintained branch. |
| !9550 | merged | Deep | John Thacker persists pcapng NRB records and replays IPv4/IPv6 callbacks during redissection. |
| !9549 | merged | Scanned | Metamako trailer field-label clarification. |
| !9548 | merged | Deep | John Thacker fixes packet-list redraw by clearing selection, not current index. |
| !9547 | merged | Scanned | Maintained-branch backport of !9548 selection fix. |
| !9546 | merged | Scanned | Second maintained-branch backport of !9548. |
| !9545 | merged | Scanned | MSYS2 build selects a precise source commit instead of branch tip. |
| !9544 | merged | Deep | John Thacker marks GTP request/response FT_FRAMENUM relation types for UI navigation. |
| !9543 | merged | Scanned | Removes obsolete WiX component reference. |
| !9542 | merged | Deep | CIP cross-packet connection analysis; Gilbert Ramirez corrected the expert-item placement. |
| !9541 | merged | Discussion-focused | Qt AUTHORS resource size/startup/documentation tradeoff debated by João Valverde and Roland Knall. |
| !9540 | merged | Deep | John Thacker replaces hardcoded Follow type switch with registered follower object. |
| !9539 | merged | Scanned | Embeds GUI license and acknowledgement resources; updates installers. |
| !9538 | merged | Deep | Deduplicates per-frame dependencies, but quadratic list scan caused severe CPU regression; corrected by !9622. |
| !9537 | merged | Scanned | Moves installed CSS to documentation destination. |
| !9536 | merged | Scanned | Compiler version check backport. |
| !9535 | merged | Scanned | Companion compiler version check backport. |
| !9534 | merged | Scanned | HTTP/2 SETTINGS parser handles a missing session pointer. |
| !9533 | merged | Scanned | PDML documentation moves to DOCDIR with path/URL follow-up. |
| !9532 | merged | Scanned | ENIP request forms do not carry CPF payload of corresponding List responses. |
| !9531 | merged | Deep | John Thacker removes FOLLOW_TCP-specific fragment cleanup in favor of initialized nullable lists. |
| !9530 | merged | Deep | TDS SSPI token length fix and nested dissection; John Thacker questions GSS-API fallback versus SPNEGO BER. |
| !9529 | merged | Scanned | Compiler-version logic backport. |
| !9528 | merged | Scanned | Removes stale WiX dftest feature reference. |
| !9527 | merged | Deep | DHCP BASE_CUSTOM numeric time formatting works in custom columns and tree. |
| !9526 | closed | Discussion-focused | Machine-readable TShark conversation statistics proposal superseded by !19329 per Michael Mann; not implementation precedent. |
| !9525 | merged | Scanned | RPM excludes nonportable/redundant documentation files. |
| !9524 | merged | Scanned | Falco extcap and dissector scan user plugin directory in addition to system location. |
| !9523 | merged | Scanned | Logray AppImage launch wrapper supports symlinked command-line tools. |
| !9522 | merged | Scanned | 3.6.12 release-version preparation. |
| !9521 | merged | Scanned | 4.0.4 release-version preparation. |
| !9520 | merged | Discussion-focused | SMB2 lock fields; reviewer spelling correction implemented later in !9588. |
| !9519 | merged | Discussion-focused | H.261 encoding fix; Martin requested ENC_NA consistency, reviewer caught syntax typo. |
| !9518 | merged | Scanned | 3.6.11 release artifacts. |
| !9517 | merged | Scanned | 4.0.3 release artifacts. |
| !9516 | merged | Deep | Reverts broken O-RAN subtree introduced by !9512; corrective merge, not a feature. |
| !9515 | merged | Scanned | NFS IO_ADVISE hint mask gets the operation subtree. |
| !9514 | merged | Discussion-focused | Removes developer-only dftest from installers and man pages; TShark filter validation discussed. |
| !9513 | merged | Deep | Moves HTML help into DOCDIR; adjusts CMake, package manifests and runtime URLs, flags macOS testing. |
| !9512 | merged | Deep | O-RAN FT_STRING with invalid nonzero mask; John Thacker suggested FT_NONE; merged without CI then reverted !9516. |

## Durable conclusions promoted

- **Metadata and redissection:** !9550 demonstrates that pcapng NRBs must own parsed names and replay them when a new resolver/dissection context is installed. This does not by itself solve output NRB copying.
- **Field semantic correctness and CI:** !9512's nonzero `FT_STRING` mask was challenged by John Thacker and promptly reverted by !9516 after an untested merge. A structural grouping item is not a fake masked string; prefer an appropriate `FT_NONE` presentation when applicable.
- **Presentation contracts:** !9527 uses `BASE_CUSTOM` for human-readable DHCP times in custom columns; !9544 registers request/response frame-number roles.
- **State/UI extensibility:** !9548 and two backports distinguish Qt current from selected index; !9540/!9531 replace closed Follow protocol type branches with actual registered follower and nullable fragment state.
- **Protocol boundaries:** !9554 respects USB address-reset lifetime; !9530's post-merge John Thacker comment identifies SPNEGO ASN.1 BER as a likely TDS SSPI payload that should not be routed to generic GSS-API without evidence.
- **Correctness versus performance:** !9538's linked-list uniqueness checks caused a severe reported capture slowdown; later !9622 fixes the data structure. A merge can be superseded technically by its own corrective follow-up.
- **Installer/data layout:** !9559's personal-extcap relocation required a later runtime discovery fix !9566; !9513's documentation move required package manifests *and* help URL consumers.
- **Superseded work:** closed !9526 was replaced by !19329 per Michael Mann, so it is review context rather than accepted code.

## Next-run invariant

Build the actual already-reviewed set from all current tracking again; do not assume any numeric interval is wholly reviewed based on filenames. Preserve the historical !17571–!17620 batch. Resume from the highest unreviewed corpus record, expected !9511 if the corpus and review tracking remain unchanged.
