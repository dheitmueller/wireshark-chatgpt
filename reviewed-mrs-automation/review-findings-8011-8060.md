# Wireshark MR review findings: 8011-8060

Corpus snapshot: `dheitmueller/wireshark-corpus-mrs@ddcaa22b51c68f594e425a23388c3a2086813054`.

Merged MRs are treated as accepted evidence; closed/superseded work is explicitly down-weighted. High-authority maintainer feedback, particularly Guy Harris, is called out where it materially shapes the lesson.

| MR | Outcome / depth | Review finding |
|---|---|---|
| 8060 | merged release / scanned | Wireshark 3.4.16 release assembly; release metadata and security/bug-note rollup, no distinct new convention. |
| 8059 | merged release / scanned | Wireshark 3.6.8 release assembly; no distinct new engineering convention. |
| 8058 | merged master / corroboration | Gerald Combs fixes BACapp's fixed text buffer indexing so the write clamp leaves room for termination; ordinary fixed-buffer bounds discipline. |
| 8057 | merged master / scanned | UAT buffer callback signedness cleanup; local warning fix. |
| 8056 | merged master / scanned | Qt conversion warning fixed by explicit narrowing for `QString::repeated()`; local type/API cleanup. |
| 8055 | merged master / scanned | Broad clang warning cleanup makes byte comparisons use unsigned-byte types/casts; mechanical API typing cleanup. |
| 8054 | merged release / corroboration | Release-4.0 backport of the ExportDissectionDialog lifetime fix from 8038. |
| 8053 | merged master / discussion | Jaap Keuter caught an unrelated nghttp2 dependency-upgrade commit in an ISAKMP MR; it was removed before merge. Keep MR/topic-branch scope focused. |
| 8052 | merged master / scanned | OPC UA `tvb_memeql` warning casts; local typing cleanup. |
| 8051 | merged master / scanned | Corrects stale function documentation; no broader rule. |
| 8050 | merged master / deep | Pascal Quantin explicitly notes that one filter abbreviation cannot mix `FT_INT8` and `FT_UINT64`. Accepted code gives incompatible encodings distinct abbreviations. |
| 8049 | merged master / deep corroboration | John Thacker fixes UTF-8 label truncation by using the documented one-past-end behavior of `g_utf8_prev_char()`; reinforces encoding-safe truncation. |
| 8048 | merged master / scanned | Adds progress UI to Expert Information dialog; UI-local feature. |
| 8047 | merged master / scanned | Reduces extcap capture-error dialog noise and strips logging prefixes for UI display; useful local UI/log cleanup, but the string-format coupling is not promoted as architecture. |
| 8046 | merged master / scanned | Debian CI/package compression and ccache invocation update; build-local. |
| 8045 | merged release / scanned | Prepares 3.4.16 release notes; no distinct convention. |
| 8044 | merged release / scanned | Prepares 3.6.8 release notes; no distinct convention. |
| 8043 | merged master / deep | Migration to named dissectors triggered a startup assertion on a duplicate registry key. Registered dissector names are global identifiers; broad migrations need runtime registration validation, not only compilation. |
| 8042 | merged master / scanned | Removes clang-reported dead stores; mechanical warning cleanup. |
| 8041 | closed / down-weighted | Draft nghttp3 Windows dependency addition; later declared unnecessary because later work superseded it. Not accepted design evidence. |
| 8040 | closed / down-weighted | Early MiWi submission. Alexis La Goutte requested rebase, representative pcap, and a topic branch rather than protected master; work continued later in 15844. Submission-process evidence only. |
| 8039 | merged master / scanned | Spelling/filter-text cleanup; no new rule. |
| 8038 | merged master / deep, high-authority discussion | John Thacker's Qt 6.3 use-after-free workaround connects earlier to `filesSelected`. Guy Harris challenged whether the signal ordering is guaranteed; Tomasz Moń traced the actual hazard to nested `processEvents()` processing DeferredDelete. Strong lifetime/event-loop guidance. |
| 8037 | merged master / scanned | ProgressFrame minimum-size UI fix. |
| 8036 | merged release / corroboration | HTTPS backport for make-manuf IEEE downloads. |
| 8035 | merged release / corroboration | HTTPS backport for make-manuf IEEE downloads. |
| 8034 | merged release / corroboration | HTTPS backport for make-manuf IEEE downloads. |
| 8033 | merged master / scanned | Missing newline warning cleanup in generated/header path. |
| 8032 | merged release / corroboration | Backport of F5 trailer heuristic infinite-loop fix; reinforces parser progress/no-progress requirements. |
| 8031 | merged release / corroboration | Backport of F5 trailer heuristic infinite-loop fix. |
| 8030 | merged release / corroboration | Backport of F5 trailer heuristic infinite-loop fix. |
| 8029 | merged master / scanned | make-manuf switches authoritative IEEE downloads from HTTP to HTTPS; tooling/security maintenance. |
| 8028 | merged master / scanned | macOS Homebrew setup changes Qt dependency from Qt 5 to Qt 6; dependency baseline maintenance. |
| 8027 | merged release / scanned | Release backport fixing ExportObjectModel ownership/leak. |
| 8026 | merged master / discussion-focused | Adds ISO15765 support over PDU Transport; unrelated newline-warning change was split to 8033. Useful scope hygiene but mainly protocol feature work. |
| 8025 | merged release / corroboration | Release-4.0 backport of Guy Harris's AppleTalk/DSI state cleanup in 8024. |
| 8024 | merged master / deep, high-authority | Guy Harris removes redundant `command` state from an AppleTalk/DSI handoff structure and returns the transaction object that already owns the value. Prefer authoritative state objects over duplicate context copies. |
| 8023 | merged release / deep, high-authority | Guy Harris makes Frame expert diagnostics run even when Frame fields are not referenced and tree generation is skipped. Tree-demand optimization must not suppress semantic diagnostics. |
| 8022 | merged release / deep, high-authority corroboration | Guy Harris introduces repair for impossible TVBuff state where reported length is less than captured length, preserving the reported/captured invariant. Later notebook evidence is stronger, so retained as corroboration. |
| 8021 | merged release / scanned | Removes obsolete SCTP NONCE_SUPPORTED support on release branch. |
| 8020 | merged master / scanned | Removes obsolete SCTP NONCE_SUPPORTED support. Protocol-specific standards cleanup. |
| 8019 | merged release / scanned | Automatic generated-data refresh. |
| 8018 | merged master but superseded / negative architecture evidence | Generic save/restore of current conversation elements caused regressions. John Thacker explicitly recommended reverting the broad mechanism and handling semantic multi-PDU boundaries where the parent knows sibling versus nested structure. |
| 8017 | merged release / corroboration | Release-3.6 backport of TVBuff reported-length repair. |
| 8016 | merged master / scanned | Automatic generated-data/translation refresh. |
| 8015 | merged release / scanned | Automatic generated-data refresh. |
| 8014 | merged release / scanned | Automatic generated-data refresh. |
| 8013 | merged master / deep | John Thacker resets transient conversation context between independent PPP raw-HDLC sibling frames. Discussion distinguishes sibling PDUs from nested encapsulation and motivates boundary-local resets. |
| 8012 | merged master / deep | John Thacker decodes percent-decoded form data as valid UTF-8 before storing it in `FT_STRING`; malformed bytes otherwise poison JSON/XML output. Presentation escaping remains separate from semantic field value. |
| 8011 | merged release / corroboration | Guy Harris AppleTalk/DSI naming/context cleanup backport; terminology follows actual transaction semantics. |

## Promoted durable findings

The strongest new/corroborating lessons from this batch are recorded in `conventions-8011-8060.md` and promoted where appropriate into the topical notebook: nested Qt event-loop lifetime hazards (8038), sibling-PDU conversation context (8013 with 8018 as negative evidence), semantic expert-info independence from tree-demand optimization (8023), named-dissector registry uniqueness (8043), text decoding before FT_STRING/export (8012), and incompatible field-type/filter-abbreviation handling (8050).
