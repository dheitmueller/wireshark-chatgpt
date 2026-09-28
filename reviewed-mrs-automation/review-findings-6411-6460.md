# Review findings for Wireshark MRs !6411-!6460

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

| MR | Outcome | Finding |
| ---: | --- | --- |
| !6460 | merged | HTTP offset fix stable backport. |
| !6459 | merged | HTTP offset fix stable backport. |
| !6458 | merged | RESPv2 dissector; sample capture, release-note, field-type and warning review. |
| !6457 | merged | RPM build dependencies. |
| !6456 | merged | Qt minimum-version compatibility. |
| !6455 | merged | Gcrypt tuple-version stable backport. |
| !6454 | merged | HTTP follow tap must start at the logical message offset. |
| !6453 | merged | Align plugin and core registration scanners. |
| !6452 | merged | Hidden UI state must not block capture; Qt baseline review also mattered. |
| !6451 | merged | Radiotap generator portability. |
| !6450 | merged | RDMA numeric display base. |
| !6449 | merged | Skip unnecessary single-fragment reassembly. |
| !6448 | merged | Optional packet-list sorting. |
| !6447 | merged | TLS reuses TCP composite reassembly identity. |
| !6446 | merged | SMC-Rv2 and reserved fields. |
| !6445 | merged | Compare versions as integer tuples, not floats. |
| !6444 | merged | Variable-cardinality jump fixups in display-filter code generation. |
| !6443 | merged | Do not reuse stale derived strings from mutable syntax-tree nodes. |
| !6442 | merged | Radiotap bit/TLV representation. |
| !6441 | closed / unmerged | Superseded dependency-heavy version parsing; Windows CI failure. |
| !6440 | merged | Composite TCP reassembly key; complete equality and lifetime-aware key copies. |
| !6439 | merged | Document TCP desegmentation limitations. |
| !6438 | merged | Guy Harris stable close-result API backport. |
| !6437 | merged | Keep ABI diagnostic artifacts on failure. |
| !6436 | merged | Continue fixes in the same MR; amend/rebase and repush. |
| !6435 | merged | Guy Harris stable close-result API backport. |
| !6434 | merged | Use NULL for a redundant header-field blurb. |
| !6433 | closed / unmerged | Rejected Qt reselection workaround; model should own mutation and notifications. |
| !6432 | merged | Guy Harris: return finalization-derived reload state from dump close. |
| !6431 | merged | Namespace exported Bluetooth globals. |
| !6430 | closed / unmerged | Rejected early prediction of reload state; superseded by !6432. |
| !6429 | merged | EAP leak fix. |
| !6428 | merged | Guy Harris: a crash guard or workaround is not necessarily a root-cause fix. |
| !6427 | merged | Test-guide correction. |
| !6426 | merged | ITS formatting; generated/template warning and squash review. |
| !6425 | merged | Wiretap Doxygen cleanup. |
| !6424 | merged | Elasticsearch mapping compatibility. |
| !6423 | merged | Automatic master data refresh. |
| !6422 | merged | Automatic release-3.6 data refresh. |
| !6421 | merged | Automatic release-3.4 data refresh. |
| !6420 | merged | Bluetooth registry refresh. |
| !6419 | merged | Extcap presence-only option must be a flag rather than a boolean-with-value. |
| !6418 | merged | CFM control-flow cleanup and invalid-value defaults. |
| !6417 | closed / unmerged | Unmerged source-tree relocation; no accepted precedent. |
| !6416 | merged | Freedesktop resource relocation. |
| !6415 | merged | Set output parameters on every normal early return. |
| !6414 | merged | Correct bit API ordering and encoding semantics. |
| !6413 | merged | Data-as-text also updates Info. |
| !6412 | closed / unmerged | Superseded combined ID3/UTF-16 work; reviewers favored splitting reusable core work. |
| !6411 | merged | Track exported symbol in Debian ABI manifest. |

Merged work is accepted evidence. Closed !6441, !6433, !6430, !6417, and !6412 are supporting or superseded evidence only. High-authority Guy Harris evidence is concentrated in merged !6432, !6435, !6438 and review discussion in !6430 and !6428. Validation: 50 rows, 50 unique MR numbers, exact descending set !6460 through !6411.
