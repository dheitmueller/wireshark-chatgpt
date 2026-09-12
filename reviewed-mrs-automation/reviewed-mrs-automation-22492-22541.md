# Wireshark MR review automation ledger: !22492–!22541

- **Corpus:** `dheitmueller/wireshark-corpus-mrs`
- **Corpus commit:** `9e52bc78659a888d4eb624984ee1a886a40d959f`
- **Review direction:** descending from newest available toward older MRs.
- **Selection method:** rebuilt the already-reviewed set from the available files under `reviewed-mrs-automation/` plus `reviewed-mrs.md`, including the historical !17571–!17620 batch; then selected the 50 highest-numbered corpus MRs not present in that set. Candidate numbers were checked individually rather than inferring coverage from numeric ranges.
- **Exact reviewed set (50):** !22541, !22540, !22539, !22538, !22537, !22536, !22535, !22534, !22533, !22532, !22531, !22530, !22529, !22528, !22527, !22526, !22525, !22524, !22523, !22522, !22521, !22520, !22519, !22518, !22517, !22516, !22515, !22514, !22513, !22512, !22511, !22510, !22509, !22508, !22507, !22506, !22505, !22504, !22503, !22502, !22501, !22500, !22499, !22498, !22497, !22496, !22495, !22494, !22493, !22492.

## Per-MR review notes

| MR | Classification | Review result |
|---:|---|---|
| !22541 | Down-weighted | Closed/unmerged compatibility attempt for the 4.6 expert-field ABI break. Useful historical context, but not promoted because the workaround was not accepted. |
| !22540 | Scanned | Merged automatic data/update MR; no durable new engineering convention beyond existing generated-data maintenance practice. |
| !22539 | Scanned | Stable-branch automatic update; no substantive human review or new convention. |
| !22538 | Scanned | Stable-branch automatic update; no substantive human review or new convention. |
| !22537 | Corroboration | Merged John Thacker `-Wpointer-sign` cleanup; reinforces matching byte/string pointer types to their semantic API contracts. |
| !22536 | Corroboration | Continued merged pointer-sign cleanup; no distinct rule beyond existing signedness/type guidance. |
| !22535 | Corroboration | Continued merged pointer-sign cleanup; no distinct rule beyond existing signedness/type guidance. |
| !22534 | Corroboration | Continued merged pointer-sign cleanup; no distinct rule beyond existing signedness/type guidance. |
| !22533 | Corroboration | Merged pointer-sign cleanup plus reuse of the shared IANA encoding registry for SRVLOC; reinforces shared authoritative registry reuse. |
| !22532 | Deep / promoted | Merged ESTA manufacturer-ID relocation/generator. Gerald Combs required a minimum-count sanity check for downloaded registry data, preferred the Python standard library over an unnecessary dependency, and requested explicit UTF-8 decoding. Promoted to `generated-registry-conventions.md`. |
| !22531 | Deep | Merged DIAMETER grouped-AVP presentation cleanup after extended discussion. Accepted design removes a redundant child subtree while retaining a generated filterable item and appends useful child information to the grouped AVP. Corroborates existing protocol-tree/filter-semantics guidance without a new broad rule. |
| !22530 | Corroboration | STMP cleanup replaces raw pointer/string handling with a typed TVBuff string helper; reinforces using purpose-built TVBuff extraction APIs. |
| !22529 | Corroboration | Merged pointer-sign/Coverity cleanup; no new convention. |
| !22528 | Corroboration | Moves internal UAT/preference includes out of Qt public headers into implementation files; reinforces header dependency hygiene. |
| !22527 | Corroboration | Merged pointer-sign cleanup; no new convention. |
| !22526 | Scanned | Guy Harris-authored/merged PeekTagged refactoring generalizes a block skipper and uses character storage for ASCII data; useful implementation cleanup, no broader new rule. |
| !22525 | Corroboration | John Thacker-authored/merged SSH cleanup keeps byte arrays consistently `uint8_t *`; reinforces semantic signedness consistency. |
| !22524 | Scanned | OTP internal-linkage cleanup; routine encapsulation improvement, no new durable convention. |
| !22523 | Corroboration | Merged pointer-sign cleanup; no new convention. |
| !22522 | Corroboration | Merged pointer-sign cleanup; no new convention. |
| !22521 | Scanned | Stable Windows Arm64 CI correction for `QT_HOST_PATH`; platform-specific build maintenance. |
| !22520 | Corroboration | Merged pointer-sign cleanup; no new convention. |
| !22519 | Deep / promoted | NATS failed on older MSVC because `{}` aggregate initialization is C23-era syntax. John Thacker explicitly required `{0}`, preserving zero-initialization while staying compatible and keeping review scope narrow. Promoted to `c-portability-conventions.md`. |
| !22518 | Scanned | Removes a long-broken macOS Logray action from release-4.4 where newer branches already provide relevant Stratoshark coverage; maintenance only. |
| !22517 | Corroboration | Corrects the CMake inventory of ASN.1-generated dissectors so warning policy can distinguish generated source that must be fixed in `asn2wrs.py`; reinforces generated-source ownership. |
| !22516 | Corroboration | IEEE 802.15.4 byte arrays converted to `uint8_t *`; reinforces byte-buffer type consistency. |
| !22515 | Corroboration | Steam-IHS reuses `tvb_get_varint()` for protobuf varints instead of maintaining a parallel implementation; reinforces common TVBuff helper reuse. |
| !22514 | Corroboration | ZigBee byte arrays converted to `uint8_t *`; reinforces byte-buffer type consistency. |
| !22513 | Corroboration | Merged pointer-sign cleanup; no new convention. |
| !22512 | Scanned | NMEA0183 enhancement for multiple tags/messages; protocol-specific functionality without broader review guidance. |
| !22511 | Corroboration | Guy Harris-authored/merged release-4.4 backport of the BER wiretap bounds fix; same lesson as master !22504. |
| !22510 | Corroboration | Guy Harris-authored/merged release-4.6 backport of the BER wiretap bounds fix; same lesson as master !22504. |
| !22509 | Scanned | Guy Harris comment-format cleanup only. |
| !22508 | Corroboration | Merged pointer-sign cleanup; no new convention. |
| !22507 | Deep / promoted | Guy Harris-authored/merged Lua portability fix. Fedora backported an API-signature change without changing the Lua release macro, so Wireshark probes the actual call signature at configure time instead of trusting version metadata. Promoted to `dependency-capability-detection-conventions.md`. |
| !22506 | Corroboration | Merged pointer-sign cleanup; no new convention. |
| !22505 | Corroboration | Merged pointer-sign cleanup; no new convention. |
| !22504 | Deep / corroboration | Master BER wiretap bounds-check fix, authored by Gerald Combs and merged by John Thacker. Reinforces existing bounded-input/checked-length guidance; stable backports are !22510 and !22511. |
| !22503 | Corroboration | Merged pointer-sign cleanup; no new convention. |
| !22502 | Scanned | ArtNet splits one shared subtree identifier into distinct `ett_` values so users can independently expand/collapse meaningful portions of large replies. Useful UI refinement, not promoted as a general architectural rule. |
| !22501 | Corroboration | Merged pointer-sign cleanup; no new convention. |
| !22500 | Scanned | Spelling cleanup only. |
| !22499 | Corroboration | Merged pointer-sign cleanup by John Thacker; no new convention. |
| !22498 | Corroboration | John Thacker replaces manual `sprintf` byte formatting with `tvb_bytes_to_str()`; reinforces purpose-built helper reuse. |
| !22497 | Corroboration | Merged pointer-sign cleanup; no new convention. |
| !22496 | Scanned | Release-4.6 backport updating the Lua Unicode feature fixture after a patch-version string change; testing maintenance. |
| !22495 | Scanned | GitLab CI Falco plugin-path correction; narrow CI maintenance. |
| !22494 | Scanned | Master Lua Unicode feature-fixture correction; narrow test-environment maintenance. |
| !22493 | Scanned | NMEA0183 copy/paste filter correction; protocol-specific bug fix. |
| !22492 | Corroboration | Guy Harris-authored release-4.6 backport replacing a packet-triggerable assertion with expert information; reinforces existing guidance that malformed capture data is a diagnosable input condition rather than an internal invariant failure. |

## Notebook changes from this batch

1. Added `dependency-capability-detection-conventions.md` from merged !22507: probe the dependency API contract actually present instead of inferring it from version metadata when downstream backports can invalidate that mapping.
2. Added `generated-registry-conventions.md` from merged !22532 and Gerald Combs's review: validate externally downloaded registries structurally and against a defensible lower bound before regenerating committed data.
3. Added `c-portability-conventions.md` from merged !22519 and John Thacker's review: under Wireshark's C17 baseline use portable `{0}` aggregate zero-initialization rather than C23 `{}`, and prefer semantic-preserving portability fixes over unnecessary behavioral changes.

The previously reviewed !17571–!17620 historical batch remains part of the already-reviewed set and was not re-reviewed here.
