# Wireshark MR review findings: 4211–4260

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

All 50 selected MRs had their corpus discussions and diffs examined. Merged work is primary evidence; closed/superseded submissions are explicitly marked and down-weighted.

| MR | Outcome | Depth | Finding |
|---|---|---|---|
| !4260 | merged | Deep | TLS JA3/JA3S fingerprints; uses add-and-return APIs while building canonical fingerprint strings. No substantive human review. |
| !4259 | merged | Scanned | Mask-width cleanup across dissectors; corroborates mask/type consistency checking. |
| !4258 | merged | Discussion-focused | CentOS 8 CMake compatibility around target_link_options; João notes MinGW Unicode linkage is a hard requirement and version gating should be localized. |
| !4257 | merged | Scanned | Fixes a false expert warning by computing the protocol-specific remaining-length rule for manufacturer messages. |
| !4256 | closed | Discussion-focused / negative | John Thacker: first_frame is not unique when multiple TCP MSPs begin in one frame; seq alone also risks wrap/reuse. Better identity is flow-scoped plus a unique instance discriminator. |
| !4255 | merged | Discussion-focused | PCEP extension; Alexis requires distinct IPv4/IPv6 filter names, underscore-safe abbreviations, and a clean component-prefixed squashed history. |
| !4254 | merged | Scanned | MinGW portability cleanup; separates target/platform assumptions from compiler-specific behavior. |
| !4253 | merged | Deep | New 5G LI dissector. Martin/Stig require unrelated TCP/reassembly changes to be split; Anders requires ASN.1 source/version provenance and actual history cleanup. Contributor supplies focused capture plus TLS key. |
| !4252 | merged | Deep | Martin Mathieson extends typed-item checker folder/path support and width modeling while fixing real EtherCAT masks; checker findings still require semantic interpretation. |
| !4251 | merged | Deep | Adds explicit profile-file registration for lazily loaded io_graphs/import_hexdump.json so profile copy/import/export sees them. |
| !4250 | merged | Deep | Roland Knall: preserve MR review history, keep local style, do not include prefs-int.h, access preferences through public prefs state, and do not edit generated translation files. |
| !4249 | merged | Scanned | Adds missing CoAP content-format registry mappings. |
| !4248 | merged | Discussion-focused | MinGW build fixes; review considers whether repeated target options belong in shared CMake helpers. Accepted code distinguishes capability/target cases rather than assuming MSVC. |
| !4247 | merged | Deep | Moves config.h to translation units and removes it from headers; reinforces configuration-macro include-order/source-boundary discipline. |
| !4246 | merged | Deep | Build sanity checks distinguish Windows target from MSVC generator/toolchain and avoid Visual Studio-specific assumptions under MinGW. |
| !4245 | merged | Deep | wsetargv.obj is MSVC-specific, not generic WIN32; compiler/toolchain predicates must not be conflated with target-OS predicates. |
| !4244 | merged | Scanned | Qt loading-time display pads minutes/seconds/milliseconds consistently. |
| !4243 | merged | Scanned | O-RAN expert-info and naming corrections, including explicit zero-extension-length diagnostics. |
| !4242 | closed | Discussion-focused / high-authority review | Guy Harris rejects architecture-derived Homebrew assumptions; Gerald suggests brew --prefix; Roland asks for explicit override for multiple installations/cross builds. Closed, so guidance is not implementation precedent. |
| !4241 | merged | Scanned | Skips unnecessary SpeexDSP package search on Windows where bundled resampler is used. |
| !4240 | merged | Scanned | Adds a Windows Doxygen search hint. |
| !4239 | merged | Discussion-focused | Roland asks commit messages to explain intent well enough to remain understandable later; testing exposed button behavior regressions before merge. |
| !4238 | merged | Deep | Generalizes display-filter RHS handling: protocol-looking identifiers are reinterpreted as unparsed values so context-specific literal parsing decides their meaning. |
| !4237 | merged | Deep | Cleans ws_getopt diagnostic writes and adds subprocess-based tests for stderr/error behavior. |
| !4236 | closed | Discussion-focused / negative | Qt teardown proposal rejected: zeroing owned objects leaks memory and deleting MainWindow earlier can invalidate callbacks. Lifecycle order must follow callback dependencies. |
| !4235 | merged | Scanned | Automatic release-3.2 registry/NEWS update. |
| !4234 | merged | Scanned | Automatic release-3.4 registry/NEWS update. |
| !4233 | merged | Scanned | Automatic master translation/registry/doc update. |
| !4232 | merged | Scanned | release-3.4 backport of the display-filter RHS reinterpretation behavior; corroborates compatibility importance. |
| !4231 | closed | Discussion-focused | Draft Follow Stream cleanup superseded by a more ambitious model/proxy architecture; not implementation precedent. |
| !4230 | merged | Deep / Guy review | Guy traces locale-reporting history and distinguishes canonical runtime locale (setlocale) from environment defaults; suggests separating concise version display from comprehensive bug-report state dumping. |
| !4229 | merged | Deep | Removes extraneous feature guards and reverts the SMI workaround from !4221 once the stale CMake-state diagnosis was established. |
| !4228 | merged | Scanned / historical counterexample | Spelling cleanup also renames registered BGP filter abbreviations. Later stronger compatibility rules should control; do not generalize this old merge into permission for cosmetic filter renames. |
| !4227 | closed | Deep / Guy review | Changing ENABLE_LUA in a reused build tree left stale CMake state; Guy recommends clearing the cache or recreating the build directory before diagnosing source defects. |
| !4226 | merged | Scanned | POD markup corrections. |
| !4225 | merged | Scanned / compatibility history | Renames IPv6 SLAAC field abbreviations as part of terminology cleanup; useful historical evidence but weaker than later filter-compatibility guidance. |
| !4224 | merged | Discussion-focused | Adds SSH HASSH/HASSHServer fields; a late review catches a missing g_free, fixed in later !4274. |
| !4223 | closed | Deep / Guy review | GNUTLS-off failure investigation shows contradictory HAVE/FOUND cache state; Guy drills into config.h and CMakeCache rather than accepting source guards. Contributor later recognizes configuration-state issue. |
| !4222 | merged | Deep | Introduces ws_getopt unit-test scaffolding with owned argv construction and subprocess tests. |
| !4221 | merged then superseded | Discussion-focused | SMI guard merged, then reverted by !4229 after reviewers concluded stale configuration was the likely cause. Treat as superseded implementation evidence. |
| !4220 | merged | Deep | Uses proto_tree_add_item_ret_uint so the displayed Floor m-line value directly controls presence of the optional Floor control port field. |
| !4219 | merged | Deep | Persists Import Hex Dump settings in per-profile JSON; review identifies missing profile-operation registration, fixed by !4251. |
| !4218 | merged | Discussion-focused | BGP-LS extension; Alexis/Martin discuss repeated filter/mask warnings and explicitly run check_typed_item_calls.py --consecutive rather than treating every warning as mechanically wrong. |
| !4217 | merged | Scanned | Fixes -Wmissing-prototypes findings with proper declarations or static linkage. |
| !4216 | merged | Scanned | Reverts a blanket MSVC C4244 warning suppression, preferring actual type fixes. |
| !4215 | merged | Deep | Fixes clang warnings in vendored getopt code, including ptrdiff_t for pointer differences and explicit braces. |
| !4214 | merged | Deep | Initial display-filter fix for RHS byte strings that collide with protocol names; semantic checker reinterprets the token instead of rejecting it as a protocol. |
| !4213 | merged | Deep / maintainer-reviewed | João replaces divergent system/getopt variants with one project-owned musl implementation; Guy confirms the goal is consistent cross-platform semantics and fewer MinGW-specific hacks. |
| !4212 | merged | Deep | Moves generic numeric/IP formatting helpers and exported symbols from epan to wsutil, reinforcing lower-layer ownership for generic utilities. |
| !4211 | closed | Discussion-focused / superseded | Initial attempt stored Import Hex Dump state in recent settings; superseded by the per-profile JSON design in merged !4219 and registration follow-up !4251. |
