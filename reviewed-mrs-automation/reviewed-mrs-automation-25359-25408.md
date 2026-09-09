# Automated Wireshark MR review: !25359-!25408

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Review direction: newest to oldest. This run reviewed exactly the 50 previously unreviewed MRs from !25408 down through !25359 after reconciling `reviewed-mrs.md` and the existing `reviewed-mrs-automation/` ledgers.

| MR | Status | Notes |
|---|---|---|
| !25408 | Scanned | release-4.4 backport of FMP/NOTIFY HandleList bounds fix; no additional lesson beyond master !25405. |
| !25407 | Scanned | release-4.6 backport of FMP/NOTIFY HandleList bounds fix; no additional lesson beyond master !25405. |
| !25406 | Scanned | release-4.6 backport of Darwin pcapng custom-option size validation; same safety lesson as master !25404. |
| !25405 | Deep | FMP/NOTIFY stops precomputing an attacker-controlled aggregate HandleList length and instead parses through normal TVB bounds checks, setting the subtree end after successful parsing. Strong parser-safety corroboration. |
| !25404 | Deep | Darwin pcapng writer validates actual string/UUID option sizes before allocation and serialization rather than reserving for an assumed size and copying arbitrary input. Strong writer-boundary validation corroboration. |
| !25403 | Scanned | Qt keeps the primary display-filter entry synchronized when filtering is initiated programmatically. Merged pragmatic UI-state repair; no broad new convention. |
| !25402 | Scanned | SECURITY.md Markdown comment syntax cleanup. Merged documentation-only fix. |
| !25401 | Deep | Final JSON hot-path optimization in the !25374/!25383 series reaches 10.2x on the stated NGAP benchmark while asserting byte-for-byte identical output and supplying profiling evidence. Reinforces the existing performance-evidence exemplar from !25548. |
| !25400 | Scanned | Qt validates empty display-filter state immediately on clear to avoid UI lag. Merged; no durable cross-subsystem lesson. |
| !25399 | Scanned | VNC passes the actual TCP context pointer into nested TLS dissection rather than NULL. Reinforces existing explicit-context-flow guidance. |
| !25398 | Scanned | Qt/KDE progress-bar stylesheet compatibility fix. Merged; platform-specific. |
| !25397 | Scanned | TLS ECH replaces unaligned typed pointer loads with wsutil/pint helpers. Reinforces existing unaligned-access portability guidance. |
| !25396 | Scanned | Automatic data/translation update; no substantive human review lesson. |
| !25395 | Scanned | Automatic data/translation update; no substantive human review lesson. |
| !25394 | Scanned | Automatic data/translation update; no substantive human review lesson. |
| !25393 | Deep | TLS secret injection includes ECH secret/config material and the outer Client Random so the resulting Decryption Secrets Block is complete for later use. Pairs with !25391's accepted redissection workflow. |
| !25392 | Scanned | Packet-range cleanup fix. Merged; no durable lesson beyond local state normalization. |
| !25391 | Deep | F5 trailer TLS secrets are fed through the existing keylog/secrets infrastructure; even secrets discovered after first-pass TLS can be injected, packets redissected, and the capture saved with a DSB. Strong architecture exemplar for normalizing late-discovered secrets into shared infrastructure instead of modifying TLS. |
| !25390 | Scanned | wsutil deprecated-function removal. Merged mechanical API cleanup. |
| !25389 | Scanned | EPAN deprecated-function removal. Merged mechanical API cleanup. |
| !25388 | Scanned | Qt FilterEdit debounce reduction backed by response-time rationale. UI-specific tuning. |
| !25387 | Discussion-focused | Martin Mathieson's cppcheck cleanup drew Jaap Keuter's question about retained commented-out code; final cleanup removed dead remnants. Useful code-cleanliness evidence, but not promoted as a separate convention. |
| !25386 | Discussion-focused (closed/superseded) | Proposed F5 TLS-secret file prescan. John Thacker encouraged reuse of `ws_memmem`; broader prescan design was ultimately not merged, while !25391 supplied the simpler accepted shared-secrets/redissection path. Treat !25391 as stronger architectural evidence. |
| !25385 | Scanned | Converts bitmask helper length parameters to unsigned semantic domain. Reinforces existing API-domain guidance. |
| !25384 | Scanned | Qt invalid filter state still notifies listeners when the detailed error changes even if the high-level Invalid enum does not. Useful local event-state nuance; not promoted broadly. |
| !25383 | Scanned | Middle JSON optimization step reaches 6.6x with byte-identical output; represented by final series result !25401. |
| !25382 | Scanned | SCTP DTLS chunk support updated to a newer draft revision. Merged protocol-specific work; no reusable review lesson extracted. |
| !25381 | Scanned | KDE theme integration for menu/toolbar backgrounds. Platform-specific UI work. |
| !25380 | Scanned | release backport removing unused DNS/TDS variables. No additional lesson. |
| !25379 | Scanned | Qt proxy-model API update for Qt 6.9/6.10. Compatibility-specific. |
| !25378 | Scanned | Removes unused deprecated QCustomPlot time-spec APIs. Compatibility cleanup. |
| !25377 | Scanned | release backport of malformed Logcat Exported-PDU TLV validation; represented by master !25360. |
| !25376 | Scanned | Removes a dissector-table hash-function member that was only needed at construction time. Small state-minimization cleanup. |
| !25375 | Scanned | second release backport of malformed Logcat Exported-PDU TLV validation; represented by master !25360. |
| !25374 | Deep | Initial JSON-output hot-path optimization establishes the series' measured large-capture benchmark and byte-for-byte output-equivalence criterion. Later !25401 is the strongest series exemplar. |
| !25373 | Scanned | EPAN deprecated-function removal. Merged mechanical cleanup. |
| !25372 | Scanned | Qt fixes DataSourceView destruction by respecting the actual QObject parent hierarchy. Merged lifetime fix; existing ownership/lifetime guidance is sufficient. |
| !25371 | Scanned | Stops installing an internal ws_mempbrk header with no public exported API. Build/API-surface cleanup. |
| !25370 | Scanned | release backport treating a NULL Exported-PDU item list as empty rather than dereferencing it. Defensive API handling; no new convention. |
| !25369 | Scanned | second release backport of NULL Exported-PDU list handling. No additional lesson. |
| !25368 | Scanned | HTTP fixes a use-after-free by testing frame state before removal frees the object. Reinforces existing lifetime/ordering guidance. |
| !25367 | Deep | Decode-As capability is derived from the authoritative `da_descriptions` registration data and the redundant `supports_decode_as` flag is removed. Strong single-source-of-truth exemplar: derive capability when existing state already determines it. |
| !25366 | Scanned | C15CH deduplicates value_strings and fixes naming. Merged cleanup; no broader lesson. |
| !25365 | Scanned | release backport switches WOWW decompression to child-TVB ownership chaining, avoiding manual lifetime management. Same lesson as master !25363. |
| !25364 | Scanned | second release backport of WOWW child-TVB ownership chaining. No additional lesson. |
| !25363 | Deep | WOWW uses `tvb_child_uncompress_zlib` so decompressed data follows TVB creation-chain lifetime instead of manual freeing. Reinforces existing ownership/lifetime guidance. |
| !25362 | Scanned | Lua debugger only defines allow-hook helpers where supported/used and treats the capability macro as Boolean. Merged warning/feature-gating cleanup. |
| !25361 | Discussion-focused (open draft) | Optional upper-layer dissection of retransmitted/reassembled TCP payloads. Author explicitly notes replay can confuse stateful subdissectors and defaults the preference off. Open/draft with no authoritative acceptance; retain only as provisional evidence. |
| !25360 | Deep | Logcat writer validates Exported-PDU TLV framing because generic UPPER_PDU input may be mismatched or malicious. Also documents that a more type-specific native representation could be less fragile. Strong boundary-validation corroboration. |
| !25359 | Scanned | Adds SECURITY.md reporting guidelines. Merged policy/documentation work; existing security-reporting notebook topic already covers the domain. |

## Durable notebook changes

- Added an architecture rule from merged !25391/!25393, with closed !25386 as negative contrast: when secret material is discovered during normal dissection, normalize it into Wireshark's shared secrets/DSB machinery and rely on redissection rather than introducing a separate capture prescan or consumer-specific TLS path unless ordering truly requires it.
- Added a single-source-of-truth rule from merged !25367: if a capability can be derived from authoritative registration/configuration data, avoid maintaining a separate Boolean flag that can diverge.
- Did not add another generic performance convention for !25374/!25383/!25401 because the notebook already records the same benchmark/profile evidence pattern from !25548.
