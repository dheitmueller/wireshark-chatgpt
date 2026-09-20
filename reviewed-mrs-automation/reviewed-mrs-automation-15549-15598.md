# Wireshark MR automation review: !15598 through !15549

Corpus commit: `130eea3c8a60902c0fc98c9e1130e73bafc6f815`

## Selection

Before selecting this batch, the already-reviewed set was rebuilt from the available `reviewed-mrs-automation/` per-run ledgers and `reviewed-mrs.md`, with the historical !17571-!17620 batch explicitly preserved and counted. The immediately preceding authoritative ledger covers !15648 through !15599. Repository-wide tracking searches found no reviewed candidate from !15598 downward that would displace an MR in this batch, and the corpus contains every MR from !15598 through !15549. Therefore the fifty highest-numbered corpus MRs not already in the reviewed set are exactly !15598 through !15549 inclusive. Selection was by exact reviewed-set subtraction, not by assuming an untracked numeric interval was already reviewed.

Weighting: merged master changes and substantive maintainer review carry the most weight; maintained-branch backports are corroboration; abandoned/superseded work is down-weighted. !15597 is closed without merge and was treated accordingly. Guy Harris-authored-and-merged !15556, !15554, !15551, and !15550 receive especially high authority weight, while later John Thacker review of !15574 is retained as an important caution about epoch arithmetic.

## Exact reviewed set

| MR | Outcome / review note |
|---|---|
| !15598 | Reviewed, merged. IPv6 comment spelling/binary-bit correction; no durable cross-cutting rule. |
| !15597 | **Closed/unmerged, down-weighted.** Superseded spelling patch. Review also noted contributor identity/branch-collaboration submission issues; useful corroboration only. |
| !15596 | **Reviewed, merged master; authored/merged by John Thacker.** Deregisters dynamically created WSLua heuristic lists on plugin reload; corroborates reload/registration lifetime guidance. |
| !15595 | Reviewed, merged. SMB TRANS2 display improvement; localized protocol presentation. |
| !15594 | Reviewed, merged; authored/merged by Gerald Combs. Expected Falco EOF path moves from warning to debug; localized logging-severity cleanup. |
| !15593 | Reviewed, merged. NSIS packaging fix for Logray plugin paths/DLL names; packaging maintenance. |
| !15592 | **Deep, merged release-3.6; John Thacker.** `DissectorTable` selector is a uint32 domain and must not pass through a possibly 32-bit signed `Lua_Integer`. Promoted with the master series to `scripting-numeric-conversion-conventions.md`. |
| !15591 | **Deep, merged release-4.0; John Thacker.** Same uint32 WSLua boundary correction as !15585/!15592; corroborating maintained-branch evidence. |
| !15590 | **Deep, merged release-4.2; John Thacker.** Same uint32 WSLua boundary correction; corroborating maintained-branch evidence. |
| !15589 | **Reviewed, merged master; John Thacker.** Tracks the configured width of `Lua_Integer` and adjusts integer/number behavior accordingly; supports the scripting numeric-domain rule. |
| !15588 | Reviewed, merged; Gerald Combs. Prints pcapng block type in hexadecimal; focused diagnostic formatting. |
| !15587 | **Reviewed, merged; Gerald Combs.** Falco cache indexes by actual event number rather than assuming contiguous append order. Useful cache-identity evidence; no separate rule promoted. |
| !15586 | Reviewed, merged; Gerald Combs. macOS GnuTLS update adds SHA-256 verification and removes obsolete GnuTLS 2.x setup logic. Dependency/setup maintenance. |
| !15585 | **Deep, merged master; John Thacker.** Source `DissectorTable` uint32 conversion fix. Promoted to `scripting-numeric-conversion-conventions.md`. |
| !15584 | **Deep, merged release-4.2; John Thacker.** Reverts a blanket `lua_pushinteger()` conversion for a value that can be fractional; review also considers supported Lua versions and 32-bit behavior. Promoted as semantic numeric-representation evidence. |
| !15583 | **Deep, merged master; John Thacker.** Same semantic revert: file-read numeric values can legitimately require `lua_pushnumber()`. |
| !15582 | **Deep, merged master; authored/merged by John Thacker.** Stale I/O Graph dialogs detach/disable model-dependent state after file close so later shared-model updates cannot mutate an old capture context. Promoted to `ui-model-state-conventions.md`. |
| !15581 | Reviewed, merged. JA4 fingerprint identifier corrections for QUIC/DTLS; protocol-specific. |
| !15580 | **Deep, merged.** Martin Mathieson catches signed/unsigned field/API mismatch in Zabbix; accepted code uses `int64_t`/`FT_INT64` after checking the source implementation's semantic domain. Corroborates semantic field typing. |
| !15579 | Reviewed, merged. Matter dissector gains direct specification-section references; useful maintainability practice, no distinct rule promoted. |
| !15578 | Reviewed, merged. New Matter code switches GLib integer aliases to C99 fixed-width/standard types; style/type modernization. |
| !15577 | Reviewed, merged; Gerald Combs authored. Adds OpenCORE AMR to macOS setup; author explicitly reports hand-testing build/install commands. Platform dependency maintenance. |
| !15576 | Reviewed, merged; Gerald Combs authored, John Thacker merged. Adds OpenCORE AMR-NB Windows dependency/package integration; platform packaging maintenance. |
| !15575 | **Reviewed, merged.** DTLS 1.3 decryption implementation includes sample capture/keylog material; good test-vector practice, already covered by existing testing guidance. |
| !15574 | **Deep, merged master; authored/merged by Guy Harris, with later substantive John Thacker review.** Adds Zigbee ZCL time encoding; later review exposes ambiguity in signed/directional epoch conversion for a post-Unix epoch. Added as a review caution to `time-value-field-conventions.md`. |
| !15573 | Reviewed, merged release-4.0. GnuTLS 3.8.4 dependency backport with branch-specific conflict resolution; packaging/backport maintenance. |
| !15572 | Reviewed, merged release-4.2. GnuTLS 3.8.4 dependency backport; corroborating packaging maintenance. |
| !15571 | **Deep, merged master; authored/merged by John Thacker.** Documents and fixes WSLua signed/unsigned/width conversion hazards across Lua versions and architectures, including undefined negative-float-to-unsigned behavior and 64-bit precision loss through `double`. Promoted to `scripting-numeric-conversion-conventions.md`. |
| !15570 | Reviewed, merged; John Thacker. UAT displays unsaved/not-yet-valid entries distinctly; focused UI state visibility. |
| !15569 | Reviewed, merged. Z21 protocol command coverage expansion from current specification; protocol feature work. |
| !15568 | **Deep, merged master; John Thacker approved/merged.** Moves TZSP dissector-table creation from handoff to protocol registration so peer handoff routines can reliably attach. Promoted to `registration-extension-point-conventions.md`. |
| !15567 | Reviewed, merged release-4.2; John Thacker. Removes a CI CMake option whose support had already been removed and which prevented Lua discovery; configuration/backport maintenance. |
| !15566 | Reviewed, merged; Martin Mathieson authored, John Thacker merged. Adds/reuses a shared true/false string for Activate/Do not activate; localized presentation consistency. |
| !15565 | Reviewed, merged master; authored/merged by John Thacker. Source CI fix removing obsolete `LUA_FIND_VERSIONS=ANY`; corroborates keeping CI options synchronized with supported build-system interfaces. |
| !15564 | **Deep, merged master; authored/merged by John Thacker.** Applies pending non-modal I/O Graph UAT changes before profile transition and tolerates absent model state. Promoted as lifecycle corroboration in `ui-model-state-conventions.md`. |
| !15563 | Reviewed, merged; John Thacker. Rejects non-positive I/O Graph interval as an API programming error; straightforward precondition enforcement. |
| !15562 | Reviewed. I/O Graph legend ordering/UI-state maintenance; no distinct cross-cutting rule. |
| !15561 | **Reviewed, merged master; authored/merged by Guy Harris.** 802.1CB passes an explicit trailer field rather than an invalid trailer sentinel that can assert when padding/trailer bytes exist; corroborates byte-boundary/tree API discipline. |
| !15560 | Reviewed. Windows GnuTLS 3.8.4 dependency update lineage; packaging maintenance. |
| !15559 | Reviewed. Spelling cleanup; no engineering convention. |
| !15558 | Reviewed. Windows nghttp2 1.61.0 maintained-branch update/backport; dependency maintenance. |
| !15557 | Reviewed. Windows nghttp2 1.61.0 maintained-branch update/backport; dependency maintenance. |
| !15556 | **Deep, merged master; authored/merged by Guy Harris.** Unknown ONC RPC programs must use a valid registered fallback protocol ID rather than protocol ID 0, which can assert in tree APIs. Promoted to `protocol-tree-ownership-conventions.md`. |
| !15555 | **Deep, merged master; authored/merged by John Thacker.** Multiple independent `UatModel`s over one mutable UAT can desynchronize row-indexed dirty/error state and crash. Accepted design shares one model and reloads it on external backing-store change. Promoted to `ui-model-state-conventions.md`. |
| !15554 | Reviewed, merged master; authored/merged by Guy Harris. Corrects swapped LSARPC BinaryString arguments; focused call-contract bug fix. |
| !15553 | Reviewed. Windows nghttp2 1.61.0 source dependency update; packaging maintenance. |
| !15552 | Reviewed. Falco Windows build restores required zlib include path; build-system maintenance. |
| !15551 | **Deep, merged master; authored/merged by Guy Harris.** Tightens MP4 time encoding name to reflect file-format semantics. Promoted to `time-value-field-conventions.md`. |
| !15550 | **Deep, merged master; authored/merged by Guy Harris.** Renames an epoch-named time encoding to an MP4 semantic encoding while keeping the old name as a compatibility alias; explicitly notes that a shared epoch does not imply a shared semantic encoding. Promoted to `time-value-field-conventions.md`. |
| !15549 | Reviewed. GitLab CI removes obsolete Docker-tag job configuration; localized CI maintenance. |

## Durable notebook changes from this run

- `scripting-numeric-conversion-conventions.md` — script/C numeric conversions must preserve the API domain's signedness, width, fractional semantics, and supported Lua-version/architecture behavior.
- `registration-extension-point-conventions.md` — dissector tables, taps, and other extension-point identities consumed by peer handoff routines belong in protocol registration, before handoff consumers run.
- `protocol-tree-ownership-conventions.md` — runtime "unknown protocol" is not an invalid registry ID; tree APIs requiring protocol ownership need a valid registered fallback identity.
- `ui-model-state-conventions.md` — centralize derived model state over a mutable backing store and detach stale views across file/profile lifecycle transitions.
- `time-value-field-conventions.md` — name time encodings for semantic formats rather than incidental shared epochs, preserve aliases when exposed, and audit conversion direction/sign for post-Unix epochs.

Notebook convention commits from this run:
- `9cffa5c044bf82174dd5bac2afc5f737ee75f419` — WSLua numeric conversions.
- `33577383cf8e12c93ae78cab28e975128f6c2f05` — registration extension-point lifecycle.
- `07a96f1b68f44e4c5f7a33360e549995da34ebf6` — protocol-tree ownership.
- `4d31c53ebb60a18f0c43efd5f63fca539ca43aee` — UI model state.
- `3b4db8aeaf6d83294cf08b7480b5d340df223793` — time encoding semantics.

The exact reviewed count for this run is 50. The next lower corpus MR, !15548, is present, so the corpus is not exhausted at this point.
