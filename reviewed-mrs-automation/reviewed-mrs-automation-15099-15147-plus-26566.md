# Automated MR review: !26566 plus !15147-!15099

Corpus commit: `1c7ad509887ee25079a7865cc62ba18cba06f49a`

Reviewed count: **50**.

Selection was produced by rebuilding the already-reviewed set from the available notebook tracking, including `reviewed-mrs.md` and the per-run ledgers under `reviewed-mrs-automation/`, with the historical !17571-!17620 batch explicitly retained. The newly scraped !26566 is higher than the old frontier, so this run is intentionally non-contiguous: !26566 followed by the next 49 unreviewed MRs, !15147 through !15099. All 50 selected MRs are merged. Stable-branch cherry-picks are treated primarily as corroboration of their master changes.

Exact reviewed MR numbers:

`!26566, !15147, !15146, !15145, !15144, !15143, !15142, !15141, !15140, !15139, !15138, !15137, !15136, !15135, !15134, !15133, !15132, !15131, !15130, !15129, !15128, !15127, !15126, !15125, !15124, !15123, !15122, !15121, !15120, !15119, !15118, !15117, !15116, !15115, !15114, !15113, !15112, !15111, !15110, !15109, !15108, !15107, !15106, !15105, !15104, !15103, !15102, !15101, !15100, !15099`

## Durable findings promoted

- **!15144 — WSLua object lifetime:** `ProtoField` now retains the original Lua value-string/unit object in the Lua registry while also holding the C-side pointer, and releases the registry reference in `__gc`. A raw native pointer is not a Lua GC root. Recorded in `scripting-object-lifetime-conventions.md`.
- **!15128 / !15132 — generated C syntax:** “Copy Bytes as C String” switched from unbounded `\xNN` escapes to bounded three-digit octal escapes, while also escaping quotes and backslashes. Source-like output must obey the target language's lexical grammar for arbitrary adjacent input. Recorded in `generated-text-output-conventions.md`.
- **!15104 — visualization data versus formatting:** I/O Graph stopped rescaling every stored point merely to show human-readable units. It now keeps values in canonical units and applies SI-prefix formatting at the axis-ticker/presentation layer. Recorded in `visualization-data-presentation-conventions.md`.
- **!15138 / !15146 — capture metadata discovery:** `reordercap` now initializes output dump parameters only after scanning all frames because IDBs, NRBs, and DSBs may be discovered in the middle of the file. Recorded in `capture-metadata-discovery-conventions.md`.
- **!15122 — profile-specific extension points:** Guy Harris authored and merged the refactor from a CLNP-specific ATN security preference/coupling to a Decode-As dissector table owned by the generic OSI security option. This is high-authority evidence that profile-specific interpretations of a generic/opaque extension belong behind a registration point rather than hard-coded into the base dissector. The current notebook records this in `registration-extension-point-conventions.md`.
- **!15108 / !15120 — lazy retap invalidation:** John Thacker's I/O Graph changes distinguish semantic stale state from view visibility: hidden graphs mark expensive derived data dirty and retap only if necessary when re-enabled. The current notebook records this in `ui-model-state-conventions.md`.

## Review notes for the complete batch

| MR | Review outcome |
| --- | --- |
| !26566 | Merged release-4.6, John Thacker authored/merged. DCT2000 NRUP padding/sub-TVB length now derives from the bounded decoded/truncated byte count rather than the original payload string length; `tvb_new_child_real_data()` ties the real-data TVB to the parent lifetime. Strong corroboration for authoritative post-validation lengths and TVB ownership. |
| !15147 | Merged master. E1AP generated ASN.1 dissector upgrade to 3GPP v17.8.0; specification/generated-source maintenance, no independent convention promoted. |
| !15146 | Merged release-4.2 backport of !15138. Confirms delayed `reordercap` dump-parameter initialization after metadata discovery. |
| !15145 | Merged master. XnAP generated dissector upgrade to v17.8.0; routine standards/generated-code refresh. |
| !15144 | Merged master, approved/merged by Anders Broman. Adds read-only WSLua `ProtoField` attributes and an explicit Lua registry reference for script-owned values exposed later; promoted to scripting object-lifetime guidance. |
| !15143 | Merged master. WSLua integer-valued attributes/globals use `lua_pushinteger`/`lua_isinteger` instead of floating `lua_Number`; Anders Broman explicitly checked Lua-version compatibility. Strong corroboration for semantic numeric representation at scripting boundaries. |
| !15142 | Merged master. Renames WSLua conversion macros after the C99-type conversion; mechanical consistency, no separate convention. |
| !15141 | Merged master. Const-ifies an immutable MSWSP property table, moving data to read-only storage; corroborates const-correctness. |
| !15140 | Merged master. Replaces another hand-written binary search in FIX with C `bsearch`; corroborates using standard algorithms when semantics fit. |
| !15139 | Merged release-4.2. `reordercap -n` no longer creates an empty output file when no rewrite is required; output-file creation is delayed until the condition requiring output is known. Useful side-effect/lifecycle evidence. |
| !15138 | Merged master, John Thacker authored/merged. Delays `wtap_dump_params_init()` until after all frames are scanned so mid-file IDBs/NRBs/DSBs are reflected in output metadata; promoted. |
| !15137 | Merged master, John Thacker authored. I/O Graph index calculation widens to `int64_t` before validating/casting to bounded consumer indices, preventing long captures/small intervals from wrapping back into earlier buckets. Corroborates widening before overflow-prone arithmetic and narrowing only after range checks. |
| !15136 | Merged master, Guy Harris authored/merged. Uses conventional `int` for a comparison-like -1/0/1 result and flags every ignored `c_warn_ver()` return for review. High-authority corroboration that meaningful validation/status returns should not be casually discarded. |
| !15135 | Merged master. Removes support for an obsolete QUIC multipath draft; protocol-version cleanup, no new convention. |
| !15134 | Merged master. NGAP generated dissector upgrade to v17.8.0; routine standards/generated-code refresh. |
| !15133 | Merged master. Continues project-owned C99/native type cleanup from GLib aliases; corroborates standard-C-type guidance. |
| !15132 | Merged release-4.2 backport of !15128; confirms the C-string escaping fix is stable-branch-worthy. |
| !15131 | Merged master. GeometryStateDialog uses normal window flags so platform-supported minimize/maximize behavior is available; UI/platform cleanup. |
| !15130 | Merged master. NR RRC generated dissector upgrade to v17.8.0; routine standards/generated-code refresh. |
| !15129 | Merged master. Protobuf URI matching gains `*` wildcard support; feature-specific matching enhancement, no durable project-wide rule promoted. |
| !15128 | Merged master, John Thacker authored/merged. Generates lexically valid C strings with bounded octal escapes and quote/backslash escaping; promoted. |
| !15127 | Merged master. Makes a NMEA0183-internal function static; corroborates keeping internal symbols private. |
| !15126 | Merged master. X2AP generated dissector upgrade to v17.7.0; routine standards/generated-code refresh. |
| !15125 | Merged master. S1AP generated dissector upgrade to v17.6.0; routine standards/generated-code refresh. |
| !15124 | Merged master. Corrects Zigbee Direct UUID mapping for join/pjoin/leave; maintainer requested and later completed a 4.2 backport. Protocol correctness plus release-maintenance corroboration. |
| !15123 | Merged master, merged by John Thacker. RakNet `open_connection_reply_2` updates persistent session encryption state because a later handshake message can disable encryption provisionally enabled earlier. Corroborates state machines committing each authoritative negotiation transition rather than treating earlier state as sticky. |
| !15122 | Merged master, Guy Harris authored/approved/merged. Moves ICAO ATN interpretation behind generic OSI security-option Decode-As dispatch and obsoletes the hard-coded CLNP preference; promoted with especially high authority. |
| !15121 | Merged master. Removes `return (0)`-style pseudo-function syntax in dissectors; style cleanup. |
| !15120 | Merged master, John Thacker authored. Disabled/hidden I/O graphs defer retapping and track whether a retap is actually pending; promoted together with !15108. |
| !15119 | Merged master. Replaces two hand-written sorted-value-string searches with `bsearch`, fixes “binary tree” versus “binary search” terminology, and documents negligible generated-code impact. Useful standard-library/performance-review corroboration. |
| !15118 | Merged master. Corrects signed/unsigned `printf` format to satisfy the actual argument type and cppcheck; type/format consistency. |
| !15117 | Merged master. Removes impossible `strlen() < 0` logic; corroborates reasoning from API return domains rather than treating static-analysis warnings cosmetically. |
| !15116 | Merged master, Stig Bjørlykke authored/John Thacker merged. Actually passes `GError **` to `g_regex_new()` before testing it, making the existing error path real rather than checking an object the API could never populate. Corroborates API error-channel contracts. |
| !15115 | Merged master. Spelling cleanup only. |
| !15114 | Merged master. Adds 802.11be EHT PHY classification in wlan_radio; protocol feature support, no general rule promoted. |
| !15113 | Merged master, Anders Broman approved. Adds IEC 61850 interpretation for MMS fields; protocol-specific semantic enrichment. |
| !15112 | Merged master. Corrects Huawei RADIUS dictionary field types/attributes. Review later cross-checks FreeRADIUS changes and related upstream issues, reinforcing that vendor dictionaries should be validated against external authoritative/upstream evidence. |
| !15111 | Merged master. Fixes a preferences-dialog pointer/data-type mismatch; corroborates matching API pointer contracts exactly. |
| !15110 | Merged master. Converts `extcap_parser` project-owned types to C99 types; corroborates standard-C-type guidance. |
| !15109 | Merged master. Continues C99-type conversion through remaining UI code; corroborates standard-C-type guidance. |
| !15108 | Merged master, John Thacker authored. Changing to/from LOAD graph semantics invalidates tapped data and therefore requires a retap; promoted with !15120 as explicit derived-state invalidation. |
| !15107 | Merged master. Removes redundant explicit zero initializers from static-storage objects because C guarantees zero initialization; language-semantics cleanup. |
| !15106 | Merged master. Documentation update for Lua 5.3/5.4 support; compatibility documentation. |
| !15105 | Merged master. Keeps the I/O Graph tracer synchronized with the selected valid/enabled UAT row while avoiding undesired fallback when the selected graph is disabled; UI-state consistency. |
| !15104 | Merged master, John Thacker authored/Anders Broman merged. Keeps graph data canonical and moves SI/unit scaling to axis formatting, avoiding expensive whole-data rescaling; promoted. |
| !15103 | Merged master. Converts frame-protocol query outputs to `bool`; corroborates standard-C-type guidance. |
| !15102 | Merged master. Adds I/O Graph TODO comments/links only; no convention. |
| !15101 | Merged release-4.2, John Thacker authored/merged. Lua uint64 string conversion uses base 0 so hexadecimal masks such as `0x...` are accepted, with tests added. Useful scripting input-domain/test corroboration. |
| !15100 | Merged master. Adds GTPv2 UP Security Policy IE decoding from TS 29.274; protocol feature support. |
| !15099 | Merged master. Replaces a dead ICAO document link with a live specification reference; documentation/provenance maintenance. |

## Weighting notes

All MRs in this run are merged, so there were no abandoned proposals to down-weight. Master MRs were weighted more heavily than their stable cherry-picks. Guy Harris's authored-and-merged !15122 and !15136 were treated as particularly authoritative. John Thacker's authored master fixes, especially !15138, !15128, !15120/!15108, and !15104, were also given strong weight; their stable backports serve mainly as corroboration.

## Notebook updates associated with this reviewed material

During this review window, the notebook contains the profile-extension-point rule for !15122 and the lazy-retap rule for !15108/!15120. This run additionally added:

- `scripting-object-lifetime-conventions.md` from !15144.
- `generated-text-output-conventions.md` from !15128/!15132.
- `visualization-data-presentation-conventions.md` from !15104.
- `capture-metadata-discovery-conventions.md` from !15138/!15146.

The corpus continues below this batch; `mr_15098.json` is present at the recorded corpus commit.
