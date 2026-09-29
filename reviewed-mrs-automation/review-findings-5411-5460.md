# Wireshark MR review findings 5411-5460

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

| MR | Outcome | Review depth | Finding |
|---|---|---|---|
| !5460 | merged | Deep | João Valverde's ASN.1 stdio conversion exposed `-Wformat-truncation` warnings. Guy Harris said the principled fix is dynamically sized output where needed, while warning triage still requires understanding real input bounds and memory scope. |
| !5459 | merged | Deep | Extends `range_string` to 64-bit values. Pascal Quantin, Anders Broman, and Stig Bjørlykke exposed downstream 32-bit assumptions in format strings, Qt, range loops, and WSLua. A shared-width migration must audit every consumer, not only the defining struct. |
| !5458 | merged | Deep | João makes unsupported MSVC and Windows SDK baselines hard CMake errors instead of soft warnings, while retaining a narrow workaround for known-compatible transitional SDKs. |
| !5457 | merged | Focused | Replaces lingering GLib assertion APIs with Wireshark `ws_assert` / `ws_assert_not_reached`, reinforcing project assertion wrappers for internal invariants. |
| !5456 | merged | Focused | Earlier companion assertion cleanup; corroborates !5457 and adds no distinct rule. |
| !5455 | merged | Focused | Fixes `not in` grammar construction by using the unary test-node setter for unary NOT. Parser AST construction must match the operator's declared arity. |
| !5454 | merged | Scanned | Gerald Combs aligns Windows CI with supported developer setup: Visual Studio's CMake and documented Qt base configuration. |
| !5453 | merged | Focused | TCPCL uses a protocol-in-name-only registration for extension dissector names so repeated extension dispatch does not pollute `frame.protocols`. |
| !5452 | merged | Deep | Documents the C11 baseline and explicitly excludes optional VLAs and Annex K interfaces. A project language baseline still needs an explicit supported-feature subset. |
| !5451 | merged | Focused | Adds Windows SDK/C11 compatibility checks and C5105 handling. Later !5458 makes the unsupported floor fatal; weighted as evolutionary evidence. |
| !5450 | merged | Focused | JSON already has the registered hf id and no longer performs an unnecessary registrar lookup just to recover the same id. |
| !5449 | merged | Deep | John Thacker fixes text-import timestamp truncation: an API expecting a one-past-end pointer must receive `base + strlen(base)`, not a length computed from `base + 1`. |
| !5448 | merged | Deep | João introduces `ws_clock_get_realtime()` with capability fallbacks and an explicit constraint that logging-context time acquisition must not call anything that can log recursively, including GLib. |
| !5447 | merged | Focused / historical | Spelling cleanup also renamed registered display-filter abbreviations. Later maintainer guidance in !5809/!5772 treats those names as compatibility surfaces, so these old renames are historical evidence rather than current naming precedent. |
| !5446 | merged | Deep corroboration | Master BLF origin for setting pcapng `OPT_IDB_TSRESOL` consistently with nanosecond Wiretap precision. Stable backport !5465 was reviewed later. |
| !5445 | merged | Deep corroboration | Master origin for making synthetic/dummy IDBs serialize non-default timestamp resolution. Stable backport !5466 was reviewed later. |
| !5444 | merged | Focused | Optimizes `wmem_strdup_vprintf()` by respecting `vsnprintf`'s returned length, copying the `va_list` for the probing call, allocating exactly once, and testing a long-output path. |
| !5443 | merged | Deep | Uses CMake's language-standard mechanism for the C11 baseline. Gerald/João discussion demonstrates that compiler, SDK, deployment target, and C/C++ modes all matter. |
| !5442 | merged | Focused | Radiotap 0-length PSDU fix makes subtree coverage and offset advancement match the actual encoded bytes. |
| !5441 | merged | Focused | SRTCP encrypted-payload parsing now requires optional `srtcp_info` before computing dependent lengths or dereferencing its members. |
| !5440 | merged | Scanned | Adds NAS-5G JSON callback decoding. Protocol-specific feature without a distinct durable convention. |
| !5439 | merged | Historical | Adds an early explicit SRTCP Decode As path and application preference. Later merged !5950 refines RTCP/SRTCP Decode As semantics, so this MR is origin/history rather than final architecture. |
| !5438 | merged | Corroboration | Stable compatibility backport for wslog CLI preference and GUI logging behavior. Lower weight than the master logging policy changes. |
| !5437 | merged | Scanned | O-RAN U-plane explanatory comments only; no reusable convention. |
| !5436 | merged | Stable backport | Release-3.4 backport of the Sysdig bounds/segfault fix from !5429, explicitly treated as security-relevant maintenance. |
| !5435 | merged | Deep | João makes stderr the default diagnostic stream so CLI pipelines are not contaminated, while retaining explicit stdout exceptions for extcaps and GUI backward compatibility. |
| !5434 | merged | Scanned | Corrects swapped BER error text only. |
| !5433 | merged | Deep | Adds display-filter all-equal syntax. Review explicitly avoids removing already released `~=` because compatibility is painful; new `!==` is added alongside it, with release-note/docs follow-up. |
| !5432 | merged | Historical / superseded | Replaces public `ssize_t` with `size_t` plus a `SIZE_MAX` sentinel for NUL-terminated regex subjects. Later merged !5515 deliberately removes that magic-sentinel contract in favor of separate semantic APIs. |
| !5431 | merged | Deep | Guy Harris disables libxml2's unused Python support on macOS after it caused an avoidable Python 2.7 framework link dependency. Build third-party components with only the features Wireshark actually needs. |
| !5430 | merged | Stable backport | Release-3.6 Sysdig update/backport from !5429. Lower architectural weight than the master fix. |
| !5429 | merged | Deep | Sysdig events can gain parameters upstream; Wireshark now stops when its local hf-index table has no corresponding entry instead of indexing beyond it and crashing. Gerald requests maintained-branch backports. |
| !5428 | merged | Stable backport | Radiotap S1G tag/length fix carried to a release branch. Master fix is !5422. |
| !5427 | merged | Focused | Adds API documentation; Anders Broman catches an IPv4/IPv6 copy-paste error. Documentation of API contracts is reviewed for semantic accuracy, not just presence. |
| !5426 | merged | Focused | Corrects display-filter grammar associativity token set and debug token strings. Keep grammar metadata synchronized with the actual token/operator inventory. |
| !5425 | merged | Deep / evolutionary | Initial C17 enablement exposed CMake-version, Windows SDK/preprocessor, and macOS deployment-target problems. Guy Harris and others diagnose toolchain components separately. Later !5443 settles on C11. |
| !5424 | merged | Deep corroboration | John Thacker removes `BASE_HEX|BASE_CUSTOM`: custom formatting is its own display mode and should not be combined with a normal numeric base. |
| !5423 | merged | Stable backport | Backport of !5419's BASE_CUSTOM column crash fix. Lower weight than the master origin. |
| !5422 | merged | Focused | Master Radiotap S1G TLV fix parses tag and length before S1G contents; author offers a synthetic conforming capture. |
| !5421 | merged | Corroboration | Removes `ENC_NA` from UTF-8 string encodings. Concrete character encoding is the contract; corroborates stronger encoding cleanup in !5509. |
| !5420 | merged | Scanned | Adds missing Windows executable resources/icons/copyright metadata for tools. |
| !5419 | merged | Deep | Master fix for custom-column crash: when a field uses `BASE_CUSTOM`, `hfinfo->strings` can represent a custom formatter and must not be interpreted as a value-string table. |
| !5418 | merged | Scanned | Automated data/translation update; no durable review convention. |
| !5417 | merged | Scanned | Automated registry/translation update; no durable review convention. |
| !5416 | merged | Scanned | Automated generated-data update, including ASTERIX source refresh; no distinct rule beyond existing generated-source guidance. |
| !5415 | closed/unmerged | Discussion-only | Documentation-location proposal was abandoned pending a broader plan. Guy Harris agreed fragmentation exists; João Valverde and Jaap Keuter favored WSDG/WSUG for broader material while accepting Doxygen for code/API annotation. Not accepted architecture. |
| !5414 | merged | Scanned | Documents existing LBM `tshark -z` statistics. |
| !5413 | merged | Focused / historical | Removes an obsolete checker escape hatch and updates encoding checks. It also contains broad field-abbreviation renames; later compatibility guidance makes such renames something to review explicitly rather than cosmetic precedent. |
| !5412 | merged | Focused | Replaces GLib `G_VA_COPY` with standard C `va_copy` after the language baseline permits it. Prefer standard-language facilities when the project baseline guarantees them. |
| !5411 | merged | Focused | Unknown display-filter tokens now return `"<unknown>"` rather than reaching an assertion after an exhaustive-switch assumption. Local robustness cleanup. |
