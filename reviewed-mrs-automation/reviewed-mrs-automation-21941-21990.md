# Automated Wireshark MR review: !21941–!21990

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed set from the available per-run ledgers under `reviewed-mrs-automation/` plus `reviewed-mrs.md`, preserving and counting the historical !17571–!17620 batch. The previous automated frontier ended at !21991. I also checked for individually recorded reviews below that frontier rather than treating ranges as implicitly complete; no isolated reviewed MR was found in !21941–!21990. These are therefore the fifty highest-numbered previously unreviewed MRs in the corpus snapshot.

Reviewed count: **50**

## Exact reviewed set

- !21990 — merged master — `Qt: Hide Go First and Last conversation packet menu items` — small UI action-state fix; no new general convention.
- !21989 — merged release backport — `TCP: Fix the Flow Counter statistics` — accepted TCP statistics correction; backport carries less independent design evidence than its master change.
- !21988 — merged master — `cql: Fix Dead Store found by Clang Analyzer` — reviewer/contributor discussion confirmed the now-unneeded code could be removed rather than retained/commented after the parser began consuming the whole message.
- !21987 — merged release backport — `GitLab CI+packaging: Reenable Falco plugins on Windows` — packaging/CI maintenance following the Falco path refactor.
- !21986 — merged — `docs: More Windows quick setup updates` — documentation synchronized with current Windows build variables; no new code convention.
- !21985 — merged — `docs: Update our release tag conventions` — records the post-October-2025 release-tag scheme; documentation rather than a reusable implementation rule.
- !21984 — merged master — `CMake: Prefer WIRESHARK_BASE_DIR and make it more convenient` — centralizes derivation of external-library/build paths from a common base rather than repeated absolute/environment-specific paths.
- !21983 — merged master — `Procmon: Fix Clang warnings` — removes dead packet-offset assignments/increments after their final use; static-analysis cleanup.
- !21982 — merged master, authored by John Thacker — `epan: Generate the filter for a FT_NONE frame bytes fallback better` — replaces hand-written byte/filter serialization with canonical `bytes_to_dfilter_repr()`, fixing the one-byte edge case and automatically inheriting future FT_BYTES representation changes; corroborates the existing canonical-representation-helper convention.
- !21981 — merged master, authored/merged by John Thacker — `winreg: StringBuf and ValNameBuf are strings` — generated IDL metadata now models UTF-16 wire content as strings rather than arrays of integers; reinforces matching field semantics to the actual representation.
- !21980 — merged master — `GitLab CI: Set FALCO_PLUGIN_DIR relative to WIRESHARK_BASE_DIR on Win` — removes a path assumption by deriving Falco plugin location from the configured build base.
- !21979 — merged release-4.6 backport, authored/merged by John Thacker — `DFS: Fix VolumeState bitmap` — avoids feeding symbolic enum names into a PIDL bitmap path that expects numeric literals; generator/input-format compatibility fix.
- !21978 — merged — `Fix a few typos.` — includes correction of a user-visible display-filter field spelling plus documentation typos; low semantic weight.
- !21977 — merged release-4.6 backport — `wslua: Setup packet record in FileHandler` — initializes `wtap_rec` at the FileHandler boundary so Lua callbacks receive required record type/encapsulation state even if scripts do not explicitly initialize it; corroborates lifecycle/default-state ownership guidance.
- !21976 — merged master — `Create an About dialog specific to Stratoshark` — refactors common Qt code so the base About dialog no longer asks which product is running; reinforces moving product behavior to application-owned implementations.
- !21975 — merged release backport — `GitLab CI: Fix the Debian Stable APT Test job` — CI maintenance.
- !21974 — merged release backport — `Qt: Don't try to update the context menu if it doesn't exist` — guards deferred/lazy UI state prior to context-menu creation; backport of the master-side lifecycle fix.
- !21973 — merged master — `GitLab CI: Fix the Debian Stable APT Test job` — CI maintenance.
- !21972 — merged master — `Qt: Don't try to update the context menu if it doesn't exist` — lazy UI objects must tolerate update calls before creation; accepted lifecycle guard.
- !21971 — merged release-4.6 backport, authored/merged by John Thacker — `pidl: NULL is not a valid value for TYPE alignment in cnf files` — PIDL TYPE alignment is a numeric field; use `0`, not a C token that Perl only coerces with warnings.
- !21970 — merged master, authored/merged by John Thacker — `WINREG,CLUSAPI: Fix imported types` — imported enum/access-mask fields must carry the correct `FT_UINT32`, base, value table, and width instead of remaining `FT_NONE`; strong corroboration of semantic field typing.
- !21969 — merged master — `WINREG: Align winreg_String to 5 bytes` — corrects PIDL/NDR64 alignment metadata based on observed capture behavior; protocol-generator correctness, not promoted as a broad standalone rule.
- !21968 — merged master — `File.c: explicitly mark a floating-point constant as float rather than double, and explicitly provide a #define for the status length.` — Guy Harris explicitly requested that the commit message describe both logically visible changes; strong submission-history evidence, but already covered by the notebook's focused/descriptive commit guidance.
- !21967 — merged master, approved/merged by John Thacker — `ber: Use 64-bit values in ber_check_value()` — removes the narrower duplicate path and checks ASN.1 value constraints consistently in 64-bit space.
- !21966 — merged master — `wslua: Setup packet record in FileHandler` — source change later backported by !21977; FileHandler initializes mandatory packet-record defaults before handing state to Lua.
- !21965 — merged release backport — `GitLab CI: Update WIRESHARK_BASE_DIR in the Windows jobs` — release-side path/base-directory maintenance.
- !21964 — merged master — `Qt: Defer updating data tab layout and scrollbars when not visible` — avoids expensive UI work for hidden data tabs and marks deferred state for later refresh.
- !21963 — merged master — `Qt: Create the hex data source context menu on demand` — lazy creation avoids paying the cost for nonvisible/nonused tabs; pairs with !21972/!21974's pre-creation guards.
- !21962 — merged master — `writecap: Fix linkage on MSYS2` — platform/static-linkage correction; no new general convention beyond existing build portability guidance.
- !21961 — merged master — `androiddump: Fix a shadowed variable name` — compiler/portability cleanup.
- !21960 — merged master — `tls-utils: Move gnutls includes to source file` — do not expose dependency headers through a public/internal header when its declarations do not require their types; corroborates existing narrow-header-dependency guidance.
- !21959 — merged — `doc: Fix bullet list in TCP ZeroWindowProbe` — documentation-only.
- !21958 — merged — `bt-dht: improve whitespace consistency` — style-only cleanup; no new rule promoted.
- !21957 — merged master — `OPSI: Remove a manually-coded binary search.` — replaces recursive project-local binary search with standard `bsearch()` and makes the static lookup table const; accepted simplification and removal of unnecessary dissection-depth manipulation.
- !21956 — merged master — `Fix C6336 warnings from PREfast.` — makes ternary/arithmetic precedence explicit rather than relying on precedence that triggered static-analysis warnings.
- !21955 — merged release backport — `Qt: QTabWidget::clear() performance improvement from Qt 6.8.2` — UI performance backport; removing tabs from the back is substantially cheaper for affected Qt versions.
- !21954 — merged master — `Qt: QTabWidget::clear() performance improvement from Qt 6.8.2` — source change for the QTabWidget clear-performance optimization.
- !21953 — merged master, authored by Gerald Combs and approved/merged by John Thacker — `tools: Add gnutls_init to our prohibited APIs` — converts the policy that GnuTLS session APIs are reserved/configured for dissection into an enforceable `checkAPIs.pl` restriction; promoted to `api-design-conventions.md`.
- !21952 — merged master — `dot11decrypt: Rewrite WEP decryption function` — replaces local RC4/CRC code with library/wsutil implementations, simplifying and speeding WEP/TKIP decryption; review also caught a `size_t` to `unsigned int` narrowing warning. Corroborates reuse of canonical/shared implementations and integer-width discipline.
- !21951 — merged release backport — `GitLab CI: Build the Rocky 9 packages with Qt 6` — packaging/CI backport.
- !21950 — merged master — `detect_bad_proto_tree_add.py: Output enhancements` — static-analysis/reporting-tool usability improvement; no new semantic convention.
- !21949 — merged — `Fix some spelling errors` — textual cleanup.
- !21948 — merged master — `NR RRC: prettify t-Service-r17 and t-ServiceStart-r18 dissection` — protocol display improvement; no additive cross-project rule.
- !21947 — merged master — `mongo: include command name in Info column` — protocol-specific Info-column usability improvement; no new general convention.
- !21946 — merged master — `wslua: Fix build on Lua 5.3` — conditionalizes thread-close behavior because Lua 5.3 and earlier depend on garbage collection; reinforces supported-version API compatibility.
- !21945 — merged master, authored/merged by John Thacker — `pidl-wireshark: Handle unions with identical cases` — separates generated switch call code from declaration/implementation emission so repeated case values can share a generated dissector implementation without duplicate definitions.
- !21944 — merged master, authored/merged by Michael Mann — `Use proto_checksum_vals for dissectors that are no longer plugins` — removes stale copied checksum value tables now that the dissectors are built in and can use the canonical shared table; corroborates avoiding duplicated shared representations.
- !21943 — merged master — `GitLab CI: Build the Rocky 9 packages with Qt 6` — packaging/CI maintenance.
- !21942 — merged master — `Bluetooth: Use wmem_strong_hash to hash UUID` — switches UUID hashing to a helper appropriate for variable-length data; accepted helper selection fix.
- !21941 — merged master, authored/merged by Michael Mann with architecture review from Guy Harris — `wsutil: Pull out "application flavor" to its own folder` — composes packet/event behavior by linking application-specific modules rather than retaining runtime product checks; Guy explicitly asked about the CLI consumers. Promoted to `application-layer-boundary-conventions.md`.

## Durable notebook changes

Two additive conventions were promoted from this batch:

1. **Link-time application composition (!21941):** when product identity selects a complete implementation, prefer application-specific modules/libraries selected by the executable over runtime `is_wireshark` / `is_stratoshark` checks in shared code. Audit both GUI and command-line consumers. This was added to `application-layer-boundary-conventions.md`.
2. **Automated API-policy enforcement (!21953):** when direct use of a low-level third-party API violates a Wireshark-wide ownership/policy rule, encode that restriction in `tools/checkAPIs.pl` or equivalent tooling rather than relying on reviewer memory. This was added to `api-design-conventions.md`.

High-value corroborating evidence was deliberately not duplicated into new rules: !21982 reinforces canonical representation helpers; !21981 and !21970 reinforce semantic field typing; !21977/!21966 reinforce initialization ownership; !21960 reinforces narrow header dependencies; !21952 and !21944 reinforce reuse of shared implementations/data; and Guy Harris's !21968 feedback reinforces descriptive commit history.

All fifty MRs in this batch were merged; release/backport MRs were given less independent architectural weight than their master-side source changes.