# Review findings: !7411-!7460

Corpus: `dheitmueller/wireshark-corpus-mrs@ddcaa22b51c68f594e425a23388c3a2086813054`

Merged work is primary evidence; closed work is down-weighted.

| MR | Outcome | Depth | Finding |
|---|---|---|---|
| !7460 | merged | Scanned | lrexlib CMake include-path follow-up. |
| !7459 | merged | Discussion | O-RAN block-FP cleanup separates digital power scaling from decompression; Martin Mathieson caught a Windows warning-as-error type mismatch before approving the correction. |
| !7458 | merged | Scanned | Display-filter `abs()` semantic checking rejects literal-only LHS input instead of assuming a field and crashing. |
| !7457 | merged | Deep | John Thacker moves Diameter command names into the registered field `value_string`, making resolved/unresolved values work in custom columns instead of only appended tree text. |
| !7456 | merged | Discussion | John Thacker removes long-dead pre-draft Diameter code; a Coverity warning on untouched code was investigated rather than blindly changed. |
| !7455 | merged | Scanned | WSLua argument-definition macros are aligned with function names. |
| !7454 | merged | Deep | John Thacker persists the last validated HTTP chunk boundary so repeated desegmentation resumes from prior progress instead of rescanning and becoming O(N²); the checkpoint is relative to message start. |
| !7453 | merged | Deep | AUTOSAR DLT support received substantial Guy Harris/Tomasz Moń review: sample capture requested; silent byte skipping rejected; capture-sized allocations handled fallibly; small integer hash keys/values simplified; ambiguous DLT naming qualified. |
| !7452 | merged | Scanned | TECMP 1.7 flags/field updates plus expert warning for invalid FlexRay header CRC overflow. |
| !7451 | merged | Scanned | UDS 2020 identifiers/names updated without claiming unsupported service decoding. |
| !7450 | merged | Scanned | lrexlib include paths corrected after Windows CI exposed generated-header lookup failure. |
| !7449 | merged | Scanned | Qt packet-list keyboard navigation avoids viewport jumps. |
| !7448 | closed/superseded | Discussion | Earlier packet-list fix carried unrelated cleanup; superseded by focused merged !7449 and !7447. |
| !7447 | merged | Scanned | Removes unnecessary FunnelStatistics method separately. |
| !7446 | merged | Discussion | RTPS security PID work accepted after detailed protocol-specific field, mask, value-table, and naming review. |
| !7445 | merged | Scanned | John Thacker fixes conversation endpoint-by-id typo. |
| !7444 | merged | Scanned | Automatic data update. |
| !7443 | merged | Scanned | Automatic data/translation update. |
| !7442 | merged | Scanned | Automatic manufacturer update. |
| !7441 | merged | Scanned | MySQL AuthSwitchRequest dissection fix; later MySQL state work is stronger evidence. |
| !7440 | merged | Scanned | CMake copy-profiles output-path comparison fix. |
| !7439 | merged | Discussion | MaxMind release-note wording broadened after Tomasz Moń noted the performance improvement was not Windows-only. |
| !7438 | merged | Scanned | Radiotap conflict cleanup. |
| !7437 | merged | Discussion | Quick Launch removal review caught stale installer checkbox and WSUG references; user-facing removals span behavior, UI, and docs. |
| !7436 | merged | Deep | Tomasz Moń fixes Windows pipe-handle ownership: parent closes duplicate child-end handles so blocking reads see EOF at child exit; after HANDLE-to-fd ownership transfer, close the fd, not the raw HANDLE. |
| !7435 | merged | Discussion | Pascal Quantin corrects MBIM bitmask membership tests and masks. |
| !7434 | merged | Scanned | Debian symbol list update. |
| !7433 | merged | Scanned | TWAMP adds RFC 8186 PTP timestamp option dissection. |
| !7432 | merged | Deep | Guy Harris: every `proto_tree_add..._ret_...` routine must return its decoded value even when no protocol tree is built. |
| !7431 | merged | Scanned | User Guide documents display-filter arithmetic. |
| !7430 | merged | Scanned | Qt display-filter expression dialog adds any/all. |
| !7429 | merged | Scanned | Doxygen warning cleanup. |
| !7428 | closed/draft | Discussion | Capture-stop-on-display-filter draft remained incomplete; Stig suggested a dedicated trigger filter. No accepted precedent. |
| !7427 | closed/draft | Discussion | Interface-address-column draft was held over UI clutter/scalability concerns. No accepted precedent. |
| !7426 | merged | Discussion | BGP FlowSpec fix; CI caught an uninitialized variable before merge. |
| !7425 | merged | Scanned | TECMP shows previously unparsed control-message payload. |
| !7424 | closed/abandoned | Discussion | John Thacker distinguishes sender-side retransmission inference from capture-side reassembly: sender retransmissions can be first-seen capture data if originals were lost. Later merged reassembly work is stronger authority. |
| !7423 | merged | Scanned | PFCP UP Function Features correction. |
| !7422 | merged | Scanned | MySQL CLIENT_QUERY_ATTRIBUTES support; later capability/state MRs are stronger evidence. |
| !7421 | closed/WIP | Scanned | Abandoned predecessor of TCP precedence experiment. |
| !7420 | merged | Scanned | Automated conflict cleanup. |
| !7419 | merged | Discussion | Qt support transition warns on 5.10/5.11 while recommending 5.12, illustrating staged baseline changes. |
| !7418 | merged | Scanned | Documentation-warning cleanup. |
| !7417 | merged | Scanned | Stable backport of PPPoE value update. |
| !7416 | merged | Discussion | PPPoE value update; contributor explicitly requested 3.6 backport. |
| !7415 | merged | Scanned | extcap example improved for conversation/endpoint and reassembly testing. |
| !7414 | merged | Scanned | WSUG typo fix. |
| !7413 | merged | Deep | João Valverde bases display-filter integer compatibility on supported conversion domains rather than exact width enum identity; integer fields can compare across widths unless overflow occurs. |
| !7412 | merged | Scanned | Display-filter macro/bookmark resources updated. |
| !7411 | merged | Scanned | Removes noisy display-filter VM debug output. |

Highest-confidence reusable evidence: !7432, !7436, !7453, !7454, !7457, and !7413.
