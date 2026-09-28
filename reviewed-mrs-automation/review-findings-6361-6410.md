# Review findings for Wireshark MRs !6361-!6410

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

| MR | Outcome | Finding |
| ---: | --- | --- |
| !6410 | merged | Gerald Combs relocates CORBA IDL inputs under epan/dissectors/corba-idl; source-tree organization only. |
| !6409 | merged | Gerald Combs centralizes protocol data under resources/protocols and updates build/license/validation paths together. |
| !6408 | merged | Clang Analyzer cleanup; initialize potentially uninitialized parser locals and remove a dead Couchbase offset increment. |
| !6407 | merged | Gerald Combs disables fuzzshark by default but explicitly enables it in a CI job so the specialized target still receives build coverage. |
| !6406 | merged | Gerald Combs adds the Falco Bridge/Logwolf foundation and an initial log conversation-filter API; later !6646 supersedes the shared packet/log registry design. |
| !6405 | merged | Packet-list scrollbar page-step workaround is scoped to macOS. |
| !6404 | merged | Companion macOS-only scrollbar correction; Windows user confirms improved behavior. |
| !6403 | merged | QUIC comment/wording typo cleanup only. |
| !6402 | merged | CIP Security values updated to a newer specification edition. |
| !6401 | merged | QUIC CIBIR transport parameter support; Ivan Nardi requested a representative trace and Alexis La Goutte supplied it. |
| !6400 | merged | EditorConfig adds repository formatting rules for Flex sources. |
| !6399 | merged | Removes an unused display-filter syntax-tree mutation helper. |
| !6398 | merged | João Valverde deprecates ~= while continuing to accept it and emitting an explicit migration diagnostic to !==; documentation and release notes are updated. |
| !6397 | merged | Ruckus RADIUS dictionary update; protocol data only. |
| !6396 | merged | Diameter S6C AVP update; protocol data only. |
| !6395 | merged | QUIC draft-34 decryption fix; Ivan Nardi independently confirmed the fix and attached a pcap reproducing the old failure. |
| !6394 | merged | IEEE 802.11 country/environment table update from the specification. |
| !6393 | merged | User's Guide grammar cleanup with ordinary copyediting review. |
| !6392 | merged | Packaging paths corrected after resource-directory relocation. |
| !6391 | merged | Adds wifidump extcap plus documentation/packaging; Roland Knall raises the useful UX idea that extcap prerequisites should be visible in configuration. |
| !6390 | merged | Documents display-filter literal-vs-field ambiguity and explicit disambiguation syntax. |
| !6389 | merged | Qt overlay-scrollbar indicator geometry fix. |
| !6388 | merged | iWARP MPA parsing is guarded before retrieving conversation state; avoids state use on packets that are not valid MPA FPDUs. |
| !6387 | merged | Display Filter Expression dialog is updated for new any/all equality operators. |
| !6386 | merged | MPEG reader learns ID3v2 image/header sizing and a shared synchsafe decoder; Windows CI exposes a signed/unsigned unary-minus warning that is then fixed. |
| !6385 | merged | Version/About output rework; John Thacker catches use of a GLib API above the supported minimum, and Guy Harris corrects an incorrect Windows-only assumption about libpcap version strings. |
| !6384 | merged | USB HID locals are explicitly initialized. |
| !6383 | merged | Companion USB HID initialization fix. |
| !6382 | closed / unmerged | Draft dark-theme implementation; useful UI discussion but no accepted implementation precedent. |
| !6381 | merged | RTPS filter display descriptions are simplified; minor spelling review. |
| !6380 | merged | Frame verdict decoding switches from manually reinterpreting raw option bytes to the typed packet_verdict_opt_t representation, eliminating the crash-prone duplicate interpretation. |
| !6379 | merged | Gerald Combs renames the image directory to resources and updates dependent paths. |
| !6378 | merged | Bluetooth GATT state keys gain direction; John Thacker catches duplex cases whose semantic service direction is opposite the current packet direction. |
| !6377 | merged | Automatic master data/translation update. |
| !6376 | merged | Automatic stable-branch data update. |
| !6375 | merged | Automatic stable-branch data update. |
| !6374 | merged | NTP Kiss-o'-Death reference IDs gain structured display with corrected RFC attribution. |
| !6373 | merged | CI copies macOS dSYM DMGs to artifact storage. |
| !6372 | merged | Debian exported-symbol manifest update. |
| !6371 | merged | macOS dSYM bundle naming fix. |
| !6370 | merged | tshark conversation/endpoint documentation correction. |
| !6369 | merged | Jaap Keuter adds a checker that cross-validates help URLs against User's Guide anchors and fails when targets are missing. |
| !6368 | merged | Removes unused help topic actions. |
| !6367 | closed / unmerged | Proposed =~ alias for contains is abandoned after João Valverde agrees it misleadingly suggests regular-expression semantics. |
| !6366 | merged | Accepted iWARP MPA marker-parsing fix/refactor. |
| !6365 | closed / unmerged | Closed duplicate/superseded iWARP MPA submission; lower evidentiary weight than merged !6366. |
| !6364 | merged | Guy Harris corrects pcap handling so the upper field is validated as reserved rather than interpreted as an active class namespace. |
| !6363 | closed / unmerged | Closed duplicate/superseded iWARP MPA submission; lower evidentiary weight than merged !6366. |
| !6362 | merged | Guy Harris decodes pcap linktype metadata subfields, gates FCS length on its presence bit, and propagates the FCS length into packet post-processing. |
| !6361 | merged | NVMe header-field array receives internal static linkage. |

Validation: 50 rows, 50 unique MR numbers, exact descending set !6410 through !6361. Merged MRs are weighted as accepted evidence; the four closed MRs are supporting or negative evidence only.
