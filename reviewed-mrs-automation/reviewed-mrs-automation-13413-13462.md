# Automated MR review: !13413 through !13462

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Direction: descending from the newest available previously-unreviewed MR.
- Exact reviewed count: **50**.
- Status mix in the corpus snapshot: **47 merged, 2 closed/unmerged (!13445, !13414), 1 open/unmerged (!13448)**.
- Reviewed-set construction: reconciled `reviewed-mrs.md`, the aggregate automation ledger, and the complete `reviewed-mrs-automation/` per-run inventory. The immediately preceding exact ledger (`!13463-!13512`) and the historical exact `!17571-!17620` ledger were inspected explicitly. Selection was performed from explicit MR-number tracking rather than assuming that a numeric interval was reviewed. The previously-reviewed !17571-!17620 batch remains counted.
- Frontier probes are not reviews: the previous run's lookup of !13462 did not count it; this run does not count !13412.

## Exact MRs reviewed

| MR | Corpus state | Review result |
|---:|---|---|
| !13462 | merged | Comment-only editcap typo fix; no durable convention. |
| !13461 | merged | Deep. Wiretap EMS probing kept stdio-style return values as `int` through EOF classification and distinguished ordinary probe EOF from I/O failure. Promoted to `wiretap-file-probing-conventions.md`. |
| !13460 | merged | BACapp vendor-ID/generator refresh; routine generated-data maintenance. |
| !13459 | merged | Qt geometry/splitter-state persistence work; useful implementation evidence, no new durable rule beyond existing UI-state guidance. |
| !13458 | merged | Deep. QUIC coalescing recognition now respects handshake/key-availability phase: Initial/0-RTT cannot be followed by same-connection 1-RTT short-header data in the same datagram. Corroborates existing negotiation/single-pass state guidance. |
| !13457 | merged | ICMPv6 router-advertisement reserved-bit correction; protocol-field fix, no new convention. |
| !13456 | merged | VP9 scalability-structure bit-shift correction; protocol-field fix, no new convention. |
| !13455 | merged | `check_tfs.py` set-based file handling; source-checker implementation cleanup, no new rule beyond existing checker guidance. |
| !13454 | merged | Qt `GeometryStateDialog` save/restore geometry support; UI persistence work, no distinct new convention. |
| !13453 | merged | IPARS dissector cleanup; no separate durable lesson. |
| !13452 | merged | Automated data/update MR; low review weight. |
| !13451 | merged | Automated data/update MR; low review weight. |
| !13450 | merged | Automated services update/failure follow-up; low review weight. |
| !13449 | merged | Automated data/update MR; low review weight. |
| !13448 | opened | DLT user-definable non-verbose dissection. Substantial but unresolved/unmerged in this corpus snapshot; down-weighted and not promoted. |
| !13447 | merged | `check_tfs.py` wrapper cleanup; corroborates source-checker maintainability only. |
| !13446 | merged | Qt packet-dialog side-by-side layout preference; UI behavior addition, no new cross-cutting rule. |
| !13445 | closed | First DLT submission, closed/unmerged and superseded by !13448; heavily down-weighted. |
| !13444 | merged | Qt splitter recent-value reset on layout changes; corroborates UI derived-state consistency. |
| !13443 | merged | Move main geometry state into common recent settings; UI state consolidation. |
| !13442 | merged | Move maximized-state handling into common recent settings; UI state consolidation. |
| !13441 | merged | Deep. MGCP removed a fixed 256-byte packet-derived stack buffer whose safety relied on the protocol specification; bounded TVBuff scanning/comparison replaces the copy. Promoted to `tvbuff-parsing-conventions.md`. |
| !13440 | merged | Stable-branch backport of invalid-range preference UI fix; accepted behavior disables OK while range syntax is invalid. |
| !13439 | merged | Stable-branch backport of invalid-range preference UI fix; same master behavior, no separate promotion. |
| !13438 | merged | Stable-branch backport of invalid-range preference UI fix; same master behavior, no separate promotion. |
| !13437 | merged | Master invalid-range preference UI fix; prevents accepting/stashing invalid data and resulting crash. Useful UI-validation evidence, retained without a new notebook file. |
| !13436 | merged | Deep/corroboration. TShark caches the interface list instead of repeatedly launching `dumpcap`; explicitly avoids repeated Windows UAC prompts. Added to `capture-discovery-cache-conventions.md`. |
| !13435 | merged | Deep. `--color` alone no longer forces TShark dissection when there is no consumer for colored/dissected output. Guy Harris suggested explicitly diagnosing the no-output case. Promoted to `cli-option-semantics-conventions.md`. |
| !13434 | merged | Capture-option handling avoids unnecessary interface-list retrieval for stdin (`-`); corroborates least-work capture discovery. |
| !13433 | merged | Adds dissector-name preference typing with validation/autocomplete and migrates users; good typed-preference API work, but no distinct rule promoted. |
| !13432 | merged | Deep/corroboration. Ensures user-specified interface descriptions override generated/system display names as intended. Part of the capture-interface lookup cleanup sequence. |
| !13431 | merged | Moves `-X stdin_descr` handling into the common user-description lookup path; corroborates centralizing interface-description semantics. |
| !13430 | merged | Qt Manage Interface dialog ownership/leak fix; routine QObject-lifetime correction. |
| !13429 | merged | Removes a useless slow `get_interface_descriptive_name()` call from TShark; corroborates avoiding redundant discovery/helper work. |
| !13428 | merged | Fixes item labels/filter metadata and IEEE 1722.1 mask widths; corroborates typed-field/mask correctness. |
| !13427 | merged | Deep scan. USB Audio control-transfer support uses interface protocol for class version and entity-ID/type state; validated against attached USB Audio v1/v2 captures. Solid protocol work but no new general rule. |
| !13426 | merged | Release-note organization keeps major.minor provenance in micro releases; documentation/release hygiene only. |
| !13425 | merged | SRT internal rejection-code name display; protocol presentation enhancement. |
| !13424 | merged | Deep. `get_interface_descriptive_name()` now accepts caller capture options instead of unconditionally rediscovering interfaces. Guy Harris explicitly discusses interface-change notifications and Refresh Interfaces as cache invalidation mechanisms. Added with !13436 to `capture-discovery-cache-conventions.md`. |
| !13423 | merged | Debian packaging simplification by Balint Reczey, merged by Gerald Combs; later follow-up split into !13761. Useful packaging evidence but existing packaging guidance already covers the durable points. |
| !13422 | merged | Zigbee Smart Energy tunnel-close timeout endianness fix/backport family; protocol correctness, no new convention. |
| !13421 | merged | Zigbee Smart Energy tunnel-close timeout endianness fix/backport family; same lesson. |
| !13420 | merged | `ws_label_strcpy` NULL-string warning hardening/backport; corroborates API precondition diagnostics. |
| !13419 | merged | Deep. Capture capability JSON compares decoded object keys with raw interface names and guarantees an error string on malformed JSON when the caller requested one. Promoted to `subprocess-ipc-lifecycle-conventions.md`. |
| !13418 | merged | Zigbee UTC-time presentation/usable numeric value improvement; field-presentation work. |
| !13417 | merged | GVCP avoids adding NULL strings to columns; backport/fix family, no new general rule. |
| !13416 | merged | GVCP avoids adding NULL strings to columns; backport/fix family. |
| !13415 | merged | GVCP avoids adding NULL strings to columns; backport/fix family. |
| !13414 | closed | Guy Harris immediately closed this wrong-branch backport (`Wrong branch.`); implementation diff is contaminated by branch mismatch, so it was counted but not used as technical evidence. |
| !13413 | merged | GitLab CI ABI-check-job removal/backport family; CI maintenance, no durable coding rule promoted. |

## Durable notebook changes from this run

1. `capture-discovery-cache-conventions.md` — added !13424/!13436 and Guy Harris's invalidation guidance; commit `035eefdbb854a59550818febf46af0d7bbd7eee2`.
2. `subprocess-ipc-lifecycle-conventions.md` — added decoded-vs-escaped JSON key semantics and parse-error output contracts from !13419; commit `62be8a55922443b7142883921d68ba98154cd11c`.
3. `wiretap-file-probing-conventions.md` — new file for stdio sentinel preservation and EOF-vs-I/O classification from !13461; commit `61fd04fb2f55f26a20a42f539ce4a96fad421d2c`.
4. `tvbuff-parsing-conventions.md` — added the protocol-maximum/fixed-buffer rule from !13441; commit `b52c09c23b046d4e004bfd2eeca7c47c4f90a795`.
5. `cli-option-semantics-conventions.md` — new file for making expensive CLI processing follow observable option semantics, with Guy Harris's diagnostic guidance from !13435; commit `b96c6343de47d6db6fb9c171db664cf77542c08c`.

## Next boundary

!13412 was not reviewed in this run. If the corpus has not advanced and no higher-numbered unreviewed MR appears after exact tracking reconciliation, !13412 is the next descending candidate.
