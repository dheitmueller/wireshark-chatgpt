# Wireshark MR review findings: !6861-!6910

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`.

Merged MRs are weighted as accepted evidence. Closed MRs are retained only where review discussion provides useful negative/history evidence.

| MR | Depth | Finding |
|---|---|---|
| !6910 | Scanned | DVB-S2X chooses the appropriate rolloff field/value table instead of displaying both interpretations. |
| !6909 | Deep | Clang Analyzer cleanup. Guy Harris caught that deleting unused assignments also deleted parser cursor increments; corrected before merge. |
| !6908 | Scanned | Qt proxy filtering prevents remote interfaces from also appearing in local/pipe interface views. |
| !6907 | Scanned | Corrects remote-interface action preselection polarity. |
| !6906 | Discussion-focused, closed | Combined Qt cleanup was closed; accepted work was split into merged follow-ups. Not implementation precedent. |
| !6905 | Scanned | Qt remote-management cleanup uses typed signal connection/lambda and removes an unnecessary slot. |
| !6904 | Discussion-focused, closed | MySQL response-length attempt; reviewer requested formatting fixes and a capture. Unmerged, so no accepted parser rule extracted. |
| !6903 | Scanned, closed | Earlier MySQL response-length attempt; unmerged and superseded by later work. |
| !6902 | Deep | SIP table-driven refactor. Jaap Keuter required semantic type information in the table rather than magic values and asked that locals be scoped where used. |
| !6901 | Deep negative, closed | Gerald Combs found `NO_PORT2_FORCE` and `NO_PORT2` differ in later conversation mutation semantics and closed the attempted simplification. |
| !6900 | Scanned | Conversation source indentation normalization only. |
| !6899 | Scanned | BTMesh display/endianness/state cleanup; merged without substantive review discussion. |
| !6898 | Scanned | PIM default-case offset correction; Alexis asked whether a backport was needed. |
| !6897 | Scanned | Automatic registry/data update. |
| !6896 | Scanned | Automatic generated-help update for stable branch. |
| !6895 | Scanned | Automatic generated-help update for older stable branch. |
| !6894 | Deep | EAP tunneling change qualifies conversation and per-packet state by protocol layer depth so nested EAP/TLS instances do not share state. |
| !6893 | Deep | Guy Harris changes USBLL's root from an anonymous subtree to an item created from the registered protocol. |
| !6892 | Deep | TEAP TLV parser advances by each TLV's declared length and keeps tree nesting aligned; contributor supplied a reproducer capture. |
| !6891 | Scanned | Stable-branch MBIM offset correction. |
| !6890 | Scanned | MBIM buffer offset correction accounts for the element-count field before RSRP/SNR data. |
| !6889 | Deep | NAS-5GS helper framing uses a minimum-length guard plus an exact signature and registers the UDP heuristic disabled by default. |
| !6888 | Scanned | Stable-branch TLS EMS/renegotiation state correction; author confirmed the test case required it. |
| !6887 | Deep | TLS/DTLS renegotiation resets session state at Client/Server Hello so EMS handshake state belongs to the new handshake. |
| !6886 | Scanned | Name-resolution plumbing gains persistent/static hostname entries across DNS, Lua, pcapng, and address-resolution APIs. |
| !6885 | Scanned | Fuzz timing stops resetting the shell-wide `SECONDS` value and records per-file elapsed time separately. |
| !6884 | Deep | New FiRa UCI dissector waited for the external LINKTYPE assignment, included sample capture and release-note coverage, and fixed portability/style warnings during review. |
| !6883 | Scanned | Stable version bump. |
| !6882 | Scanned | Stable release build metadata. |
| !6881 | Scanned | NSIS uses the exact redistributable file selected by CMake. |
| !6880 | Scanned | Removes obsolete/confusing Windows redistributable error guidance. |
| !6879 | Scanned | Stable version bump. |
| !6878 | Scanned | Older-stable version bump. |
| !6877 | Scanned | Release-note generator adds punctuation when issue titles lack it. |
| !6876 | Scanned | Older-stable release build metadata. |
| !6875 | Scanned | Stable release build metadata. |
| !6874 | Scanned | SOME/IP adds generated hidden string fields so resolved service/method/client names can be filtered without replacing numeric wire fields. |
| !6873 | Scanned | Falco Bridge validates mapping state and avoids repeated initialization. |
| !6872 | Deep | PDCP-LTE retains a frame-indexed history of changing protocol configuration so redissection uses the value applicable at the current frame. |
| !6871 | Deep | CI expands typed-item checks with the then-current label/mask/consecutive checks; review clarifies that commit-message failures require amending the commit, not editing only the MR title. |
| !6870 | Deep | Reverts a warning workaround that fails to compile with Qt 6.3; supported dependency semantics take priority over silencing one analyzer. |
| !6869 | Scanned | Fuzz harness reports elapsed time per input. |
| !6868 | Scanned | Older-stable release preparation. |
| !6867 | Scanned | Stable release preparation. |
| !6866 | Scanned | CI moves back to Clang 14. |
| !6865 | Scanned | Fuzz diagnostics show all commits from the previous 48 hours rather than guessing the single latest commit caused a failure. |
| !6864 | Deep | EAP temporary `packet_info` override switches from owning address copies to shallow borrowed copies, fixing a leak. |
| !6863 | Scanned | Automatic data/translation update. |
| !6862 | Scanned | Utility path updated after profile-resource relocation. |
| !6861 | Scanned | Automatic registry/data update. |

## Highest-confidence durable evidence

The strongest accepted lessons are !6909 (warning cleanup must preserve parser side effects), !6894 (nested protocol instances need instance-qualified state), !6893 (protocol roots should use the registered protocol item), !6884 (externally assigned capture LINKTYPE before integration), !6872 (frame-relative state history for redissection), !6870 (do not trade supported-build correctness for analyzer silence), and !6864 (address-copy mode follows destination ownership/lifetime). Closed !6901 is strong negative API evidence because Gerald Combs authored and then withdrew the simplification after finding a semantic difference.
