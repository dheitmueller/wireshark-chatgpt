# Wireshark MR review automation ledger: !25459-!25508

Reviewed on 2026-09-09, continuing newest-to-oldest.

Corpus: `dheitmueller/wireshark-corpus-mrs`
Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: consulted `reviewed-mrs.md` and the existing `reviewed-mrs-automation/` run ledgers, built the already-reviewed set, and selected the 50 highest-numbered corpus MRs not already represented. The previously reviewed !17571-!17620 batch remains counted. This run reviewed exactly !25508 down through !25459.

Weighting: merged master MRs and authoritative maintainer-authored/reviewed changes were weighted most heavily. Backports were treated mainly as corroboration. Open !25464 is provisional architecture evidence only.

| MR | State / target | Review depth | Durable result / weighting |
|---|---|---|---|
| !25508 | merged, master | Deep | Guy Harris-authored wiretap error-info fix. Ensures an internal error path supplies explanatory `err_info`; reinforces complete error contracts. |
| !25507 | merged, master | Scanned | TRDP XML validation and Coverity correction. Useful correctness cleanup; no new general rule promoted. |
| !25506 | merged, master | Deep | SMB2 replaces unchecked direct indexing into `smb2_cmd_vals[opcode]` with `decode_smb2_name()`, following Gerald Combs's recommendation. Strong evidence to use bounded/semantic lookup helpers for packet-controlled enum values. |
| !25505 | merged, release-4.4 | Scanned | Backport of Lua wiretap error-info leak fix; no additional lesson beyond master change. |
| !25504 | merged, release-4.6 | Scanned | Backport of Lua wiretap error-info leak fix; no additional lesson beyond master change. |
| !25503 | merged, master | Deep | Guy Harris-authored Lua fix: when a non-returning error API prevents cleanup, push/copy the formatted error first, release owned memory, then invoke the non-returning error. Strong error-path ownership exemplar. |
| !25502 | merged, master | Scanned | Re-enables Coverity jobs after service recovery; operational CI change, no durable coding rule. |
| !25501 | merged, master | Deep | John Thacker BLF decompression hardening rejects impossible compression ratios, grows output incrementally, checks actual decompressed size against claimed size, and uses checked arithmetic. Strong hostile-length/decompression exemplar; largely reinforces existing bounded-input guidance. |
| !25500 | merged, master | Deep | John Thacker converts `proto_data` storage from manually freed `GSList` to wmem-scoped containers after an OSS-Fuzz failure. Reinforces lifetime-aware container choice and avoiding allocator/lifecycle mismatches. |
| !25499 | merged, master | Deep | CMake checks libpcap `time_t` size on 32-bit builds. Strong corroboration that ABI-visible type width must agree across dependencies. |
| !25498 | merged, master | Scanned | O-RAN filter cleanup for duplicate/confusing filters; no new durable rule. |
| !25497 | merged, master | Deep | Large GlusterFS decoder expansion with unusually explicit evidence tiers: field layouts/source bindings were verified independently, only observed procedures were claimed as live-capture-tested, and unobserved cases were explicitly labeled source/spec-verified only. Strong submission/testing exemplar. |
| !25496 | merged, master | Scanned | cppcheck-driven dissector cleanup; no new lesson beyond existing static-analysis guidance. |
| !25495 | merged, master | Deep | O-RAN bounds fix protects `beamIds[]` indexing when packet-derived `startPrbc` exceeds expected range. Reinforces concrete backing-storage bounds independent of nominal protocol expectations. |
| !25494 | merged, master | Scanned | 32-bit `time_t` regression behavior corrected in tests and conversion error handling; corroborates cross-width testing. |
| !25493 | merged, master | Deep | Correct GLib callback signatures are provided via wrappers around C11-typed helpers; reinforces respecting callback ABI/signature contracts rather than relying on casts or incompatible function types. |
| !25492 | merged, master | Deep | GlusterFS fixes several wire-layout mistakes by checking generated XDR/source definitions and live captures. Strong source-of-truth plus capture validation exemplar. |
| !25491 | merged, master | Deep | GlusterFS consumes XDR `opaque<>` padding to the 4-byte boundary and handles an older union arm sharing the same representation. Reinforces format alignment as part of field consumption. |
| !25490 | merged, master | Scanned | `update-tx` checks for required `lupdate` tool before use; sensible tooling check, no new general rule. |
| !25489 | merged, release-4.4 | Scanned | Backport of `NSTIME_INIT_MAX` portability fix using `TIME_T_MAX`; no additional lesson. |
| !25488 | merged, master | Deep | John Thacker hardens Zigbee time conversion for 32-bit `time_t` with checked addition and documents type assumptions against current standards. Strong portable arithmetic exemplar. |
| !25487 | merged, master | Deep | GlusterFS procedure enum resynchronized with the actual wire enum; intentionally leaves newly named operations opaque until real decoders exist. Good conservative-dissection and source-of-truth exemplar. |
| !25486 | merged, master | Scanned | Corrects Diameter AVP from UTF8String to OctetString because its only valid value is raw `0xff`; reinforces choosing field types from protocol semantics, not superficial similarity. |
| !25485 | merged, master | Scanned | Break-statement indentation cleanup; no durable lesson. |
| !25484 | merged, release branch | Scanned | CI fuzz-artifact upload fix backport; no additional coding convention. |
| !25483 | merged, release branch | Deep | John Thacker strengthens HiPerConTracer heuristic with additional protocol-invariant payload bytes derived from source and testing. Strong evidence that cheap stable invariants should be used to reduce heuristic false positives. |
| !25482 | merged, master | Scanned | `format_text_internal` optimization batches printable ASCII copies. Performance implementation detail, no notebook rule promoted. |
| !25481 | merged, release-4.4 | Scanned | Large-file-support CMake ordering backport; corroboration only. |
| !25480 | merged, master | Scanned | Automated data/translation update; no durable lesson. |
| !25479 | merged, release-4.6 | Scanned | Automated data/translation update; no durable lesson. |
| !25478 | merged, release-4.4 | Scanned | Automated data/translation update; no durable lesson. |
| !25477 | merged, master | Scanned | Haiku setup package-name/dependency cleanup; platform-specific packaging detail. |
| !25476 | merged, master | Scanned | IKEv2 preference error message corrected to name the actual algorithm; no broader rule beyond accurate diagnostics. |
| !25475 | merged, master | Deep | ETW error paths now translate Win32 codes to human-readable messages, fix a mislabeled API name, and set `*err` on every failure path. Strong complete/actionable diagnostic-contract exemplar. |
| !25474 | merged, master | Deep | Adds Haiku setup and brings setup scripts under shellcheck, fixing findings in existing scripts. Corroborates extending project checkers to newly added platform tooling. |
| !25473 | merged, master | Deep | Guy Harris-authored capture portability cleanup tested on macOS, Linux, Windows, FreeBSD, NetBSD, OpenBSD, DragonFly BSD, Solaris, and Haiku; restructures platform ifdefs. Strong cross-platform validation exemplar. |
| !25472 | merged, master | Scanned | Corrects DNS RFC references; documentation/source accuracy cleanup. |
| !25471 | merged, release-4.6 | Scanned | Backport of DVB-RCS2 FCT2 bitmask correction; no additional lesson. |
| !25470 | merged, master | Scanned | Prefers C-standard printf formats to GLib variants in xgt; portability/style cleanup. |
| !25469 | merged, master | Scanned | CI error-output path made consistent; no new durable rule. |
| !25468 | merged, release-4.4 | Scanned | Backport of large-file-support ordering fix; no additional lesson. |
| !25467 | merged, master | Deep | CMake `COMPILE_DEFINITIONS` ordering fix demonstrated with a 32-bit Debian i386 container because later directory-property assignment overwrote feature-test definitions. Reinforces validating build-system semantics on the affected architecture. |
| !25466 | merged, master | Scanned | Debian setup installs package providing `setcap`; packaging-specific dependency correction. |
| !25465 | merged, master | Deep | John Thacker standardizes time printing via `intmax_t`/C printf formats and avoids assuming `long` width. Strong portable-formatting exemplar. |
| !25464 | open, master | Discussion-focused (provisional) | Proposed USB HID product-keyed subdissector table. Tomasz Moń challenged whether VID/PID alone supplies enough semantic context without CONFIGURATION/HID REPORT descriptors; discussion suggests descriptor availability is a core architectural precondition. Open/unmerged, so do not treat design as accepted. |
| !25463 | merged, master | Scanned | Removes stray CMake comment marker; no durable lesson. |
| !25462 | merged, master | Deep | `ws_verify_peercred()` portability fix avoids assuming `SO_PEERCRED` has Linux semantics everywhere, prefers `getpeereid()` where available, and isolates platform-specific implementations behind `ws_getpeereid()`. Strong semantic capability/portability abstraction exemplar. |
| !25461 | merged, master | Deep | DVB-RCS2 bitmask corrected from two bits to the protocol-defined four-bit lower nibble with standard reference and explicit synthetic-test recipe. Good spec-to-mask validation exemplar. |
| !25460 | merged, master | Scanned | UTF-8 validation optimization; performance implementation detail, no new durable rule. |
| !25459 | merged, master | Deep | CoAP OSS-Fuzz fix initializes a boolean out-parameter to its success value instead of only assigning failure cases. Reinforces total initialization of out-parameters on every return path. |

## Durable notebook promotions

This batch adds or strongly sharpens four reusable ideas:

1. **Heuristic recognition should exploit cheap, stable protocol invariants, not merely a minimal prefix.** !25483 is merged John Thacker evidence; negative/nonmatching traffic remains part of validation.
2. **Be explicit about verification tiers in MR descriptions.** !25497 distinguishes live-wire-tested operations from layouts verified only against schema/source, avoiding claims stronger than the evidence.
3. **Abstract platform semantics rather than macro spellings.** !25462 shows that a macro such as `SO_PEERCRED` can exist with different contracts across operating systems; isolate platform implementations behind one semantic helper and prefer broadly supported APIs when available.
4. **Packet-controlled enum/table indexes should go through semantic bounded lookup helpers.** !25506 follows Gerald Combs's recommendation to use `decode_smb2_name()` instead of direct indexing, eliminating an overflow from unexpected opcode values.

Other strong changes in this run mostly reinforce notebook rules already present for checked arithmetic, storage-capacity validation, allocator/scope matching, ABI type-width agreement, and actionable diagnostics.