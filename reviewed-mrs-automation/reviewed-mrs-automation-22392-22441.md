# Wireshark MR review automation ledger: !22392–!22441

- **Corpus:** `dheitmueller/wireshark-corpus-mrs`
- **Corpus commit:** `9e52bc78659a888d4eb624984ee1a886a40d959f`
- **Review direction:** descending from newest available toward older MRs.
- **Selection method:** rebuilt the already-reviewed set from all available files under `reviewed-mrs-automation/` plus `reviewed-mrs.md`, explicitly preserving/counting the historical !17571–!17620 batch; then selected the 50 highest-numbered corpus MRs absent from that set. Candidate files were checked individually in the corpus and the tracking was searched for candidate MR numbers rather than inferring coverage from numeric ranges.
- **Exact reviewed set (50):** !22441, !22440, !22439, !22438, !22437, !22436, !22435, !22434, !22433, !22432, !22431, !22430, !22429, !22428, !22427, !22426, !22425, !22424, !22423, !22422, !22421, !22420, !22419, !22418, !22417, !22416, !22415, !22414, !22413, !22412, !22411, !22410, !22409, !22408, !22407, !22406, !22405, !22404, !22403, !22402, !22401, !22400, !22399, !22398, !22397, !22396, !22395, !22394, !22393, !22392.

## Per-MR review notes

| MR | Classification | Review result |
|---:|---|---|
| !22441 | Scanned | Merged release-4.4 backport removing stray quote characters from generated enterprise names. Generated-data correctness fix; no distinct new convention. |
| !22440 | Scanned | Merged release-4.6 backport of the enterprise-name quote cleanup. No additional durable lesson beyond the master-side fix. |
| !22439 | Deep / promoted | Guy Harris-authored and merged Coverity cleanup replaces `while (unsigned_count--)` because the failed zero test still decrements and wraps the unsigned counter. Promoted to `arithmetic-safety-conventions.md`. |
| !22438 | Corroboration | John Thacker-authored Kerberos PA-SPAKE fix keeps a CHOICE selector signed while it can carry `-1`, then converts only after checking the sentinel. Reinforces semantic-domain/sentinel typing. |
| !22437 | Scanned | Merged ISOBUS year-field indexing correction. Protocol-specific decoding fix. |
| !22436 | Deep / corroboration | John Thacker-authored Snort parser change selects signed/unsigned numeric parsing according to the destination semantic domain and checks conversion errors. Reinforces bounded parsing and type-domain guidance. |
| !22435 | Scanned | Merged John Thacker units-format correction found by Coverity. Narrow display correctness fix. |
| !22434 | Corroboration | John Thacker-authored/merged NTLMSSP cryptographic error handling checks and propagates gcrypt failures. Reinforces existing failure-return checking guidance. |
| !22433 | Corroboration | John Thacker-authored/merged iLBC pointer-sign fix matches the external library's `uint8_t *` contract. Reinforces API-boundary semantic typing. |
| !22432 | Deep / corroboration | Merged NFLOG cleanup includes direct John Thacker commit-message guidance (`component: brief summary`, blank line, wrapped explanatory body) and removes tree-dependent semantic behavior. Both are already covered by notebook rules. |
| !22431 | Corroboration | John Thacker-authored/merged wsutil sign-conversion cleanup uses semantically appropriate types/casts after checks. Reinforces type-domain portability guidance. |
| !22430 | Scanned | Automatic shared data/translation update. No coding or architecture convention. |
| !22429 | Scanned | Automatic release-4.6 data/documentation output update. No durable convention. |
| !22428 | Scanned | Automatic release-4.4 data/documentation output update. No durable convention. |
| !22427 | Scanned | Release-4.4 WinSparkle CMake scoping backport. Platform/build maintenance. |
| !22426 | Scanned | Release-4.6 WinSparkle CMake scoping backport. Platform/build maintenance. |
| !22425 | Deep / promoted | Guy Harris-authored and merged `str_to_eth()` API change makes the MAC-48 object shape explicit with a pointer to a six-byte array instead of an unbounded byte pointer. Promoted to `c-type-conventions.md`. |
| !22424 | Corroboration | Michael Mann-authored/merged broad non-dissector pointer-sign cleanup. Reinforces semantic pointer/API-contract typing. |
| !22423 | Corroboration | John Thacker-authored/merged `wsgcrypt` return-value handling propagates cryptographic errors and performs correct cleanup. Reinforces failure-handling guidance. |
| !22422 | Corroboration | John Thacker-authored/merged wsutil sign-conversion fixes. No new convention beyond type-domain guidance. |
| !22421 | Corroboration | John Thacker-authored/merged `wsgcrypt` leak fix frees temporary nonce storage once the library has copied it. Reinforces ownership/lifetime cleanup. |
| !22420 | Deep / corroboration | John Thacker-authored/merged OSS-Fuzz COSEM fix moves a count into its actual unsigned domain to avoid signed-overflow UB. Later !22439 supplies the stronger loop-control lesson. |
| !22419 | Scanned | Merged O-RAN FH CUS radio-transport fragmentation support. Substantial feature, but the corpus discussion did not establish a distinct general convention beyond existing reassembly guidance. |
| !22418 | Corroboration | Release-4.4 backport of the fc00/cjdns provenance and custom-Base32 cleanup from !22415. Strengthens the accepted provenance outcome. |
| !22417 | Deep / corroboration | John Thacker-authored/merged OSS-Fuzz ReLOAD fix avoids signed-overflow UB in length processing and bounds derived lengths. Reinforces arithmetic/type-domain guidance. |
| !22416 | Corroboration | Release-4.6 backport of !22415's fc00/cjdns provenance and custom-Base32 cleanup. |
| !22415 | Deep / promoted | John Thacker-authored and merged master cleanup removes code apparently copied verbatim from cjdns with unclear GPLv3-to-GPLv2 relicensing/attribution, and notes that a protocol-specific Base32 alphabet should not masquerade as a generic wsutil Base32 implementation. Promoted to new `source-provenance-conventions.md`. |
| !22414 | Deep / promoted | Large TRDP replacement merged after prolonged review. John Thacker's recursion review requires proving monotonic parser progress and ruling out offset overflow/wrap/stalling before suppressing `misc-no-recursion`. Promoted to `parser-recursion-conventions.md`. |
| !22413 | Scanned | Merged LIN fix passes `lininfo` rather than `&lininfo` as subdissector data. Correct pointer level/API use; no separate notebook rule. |
| !22412 | Corroboration | John Thacker-authored wsutil sign-conversion cleanup also checks `vsnprintf()` failure before converting its result to a size. Reinforces checked conversion/error guidance. |
| !22411 | Down-weighted / revert | Gerald Combs-authored, John Thacker-merged revert of the Npcap/USBPcap FetchArtifacts change. The reverted design is not treated as accepted architecture. |
| !22410 | Scanned | Merged nl80211 MLO-links dissection with example output. Protocol-specific feature. |
| !22409 | Deep / corroboration | Michael Mann-authored/merged preference pointer-sign cleanup received Guy Harris review on redundant casts after types were corrected. Reinforces semantic typing and avoiding stale casts. |
| !22408 | Down-weighted / open | Still-open dissector-groups proposal. Michael Mann supported grouping but objected to disabling strong magic-number/distinct-byte heuristics by default. Useful provisional feedback, not promoted as accepted policy while unresolved. |
| !22407 | Scanned | Merged CI change enables release tests in Windows package jobs. Useful coverage improvement; existing testing guidance suffices. |
| !22406 | Corroboration | Release-4.6 wiretap cleanup replaces BSD `u_char`/`u_int` with standard C `uint8_t`/`unsigned`. Reinforces portability/type guidance. |
| !22405 | Scanned | Merged SMB cleanup removes `_U_` annotations from parameters actually used by the implementation. Local compiler/optimizer hygiene. |
| !22404 | Scanned | Merged master CMake change scopes WinSparkle discovery to the relevant Windows GUI applications. Build-system maintenance. |
| !22403 | Deep / corroboration | John Thacker-authored release backport establishes a public `wtap_module.h` boundary for built-in/third-party wiretap modules and moves module-facing declarations out of the internal header. Strongly reinforces existing public-header/plugin-boundary guidance. |
| !22402 | Deep / corroboration | John Thacker-authored/merged TPKT fix uses the protocol-declared payload as reported length and preserves truncation semantics via `FragmentBoundsError` rather than shrinking the subset. Reinforces existing TVBuff reported-vs-captured-length guidance. |
| !22401 | Deep / corroboration | John Thacker-authored/merged TIPC fix likewise uses protocol-reported length as reported length and rejects negative derived payload lengths before they can overflow. Reinforces TVBuff and arithmetic validation guidance. |
| !22400 | Scanned | Release-4.6 DECT NR Association Request channel/bitmask correction. Protocol-specific backport. |
| !22399 | Scanned | Merged master DECT NR Association Request channel/bitmask correction. Protocol-specific correctness work. |
| !22398 | Down-weighted / open | Still-open UTF-8 Packet Bytes display proposal with extensive review. Useful UI implementation discussion, but unresolved state means it is not accepted project policy. |
| !22397 | Scanned | Merged nl80211 generator source-URL/constants refresh after kernel.org access changes. Generated-registry maintenance; no additive convention beyond existing generator validation guidance. |
| !22396 | Scanned | Gerald Combs-authored/merged WiX directory/component-group fix. Packaging-specific maintenance. |
| !22395 | Deep / corroboration | New OTP dissector merged after maintainer review explicitly asked whether it had been fuzzed; contributor reported fuzzing with available captures before merge. Reinforces existing expectation to exercise new dissectors with fuzzing/sample data. |
| !22394 | Scanned | Release-4.6 GnuTLS 3.8.11 artifact update. Dependency maintenance. |
| !22393 | Scanned | Master GnuTLS 3.8.11 artifact update. Dependency maintenance. |
| !22392 | Deep / corroboration | John Thacker-authored/merged CMake change narrows suppression of pointer-sign warnings to dissectors while exposing/fixing such warnings elsewhere. Reinforces the value of keeping warnings enabled where practical rather than globally silencing type mismatches. |

## Notebook changes from this batch

1. Extended `arithmetic-safety-conventions.md` from merged master !22439: do not use unsigned post-decrement as a countdown loop condition when zero is the semantic terminal state, because the failed final test wraps the counter.
2. Extended `c-type-conventions.md` from merged master !22425: when an API consumes one fixed-size binary object, encode that shape in the C parameter type where practical instead of reducing the contract to an unbounded byte pointer.
3. Extended `parser-recursion-conventions.md` from merged !22414: suppress a recursion warning only after proving monotonic parser progress and ruling out offset overflow, wrap, or a non-advancing malformed-input path.
4. Added `source-provenance-conventions.md` from merged master !22415 (with merged stable backports !22416 and !22418): verify source provenance/relicensing rights, and do not expose a protocol-specific algorithm variant as a misleadingly generic shared utility.
5. Kept the TVBuff, signedness, failure handling, ownership, CI/testing, public-header, and fuzzing findings as corroboration where later/stronger notebook rules already cover them rather than duplicating guidance.
6. Explicitly down-weighted still-open !22408 and !22398 and the reverting context around !22411 rather than treating unresolved/reverted designs as accepted architecture.

The previously reviewed !17571–!17620 historical batch remains part of the already-reviewed set and was preserved/counts for future selection.
