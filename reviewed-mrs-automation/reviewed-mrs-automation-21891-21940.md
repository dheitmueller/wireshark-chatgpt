# Automated Wireshark MR review: !21891–!21940

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed set from all available per-run ledgers under `reviewed-mrs-automation/` plus `reviewed-mrs.md`, explicitly preserving and counting the historical !17571–!17620 batch. The preceding automated frontier ended at !21941. Sparse entries in `reviewed-mrs.md` were also checked rather than assuming numeric ranges were complete; none of !21891–!21940 was already recorded. These are therefore the fifty highest-numbered previously unreviewed corpus MRs in this snapshot.

Reviewed count: **50**

## Exact reviewed set

- !21940 — merged release backport, authored/merged by John Thacker — `PER: Don't add data sources that aren't new` — backport of !21931; corroborates registering a Packet Bytes source only for a distinct underlying `ds_tvb`.
- !21939 — merged release backport, authored/merged by John Thacker — `smb: do not attempt to dissect wfw3.11 negotiate protocol blob` — unknown/undocumented bytes are left opaque rather than falsely decoded or marked malformed; backport of !21937.
- !21938 — merged release backport, authored/merged by John Thacker — `SMB: Remove proto_tree_add_string from dissect_create_file_request` — removes a tree addition that incorrectly produced Malformed; backport-side evidence.
- !21937 — merged master — `smb: do not attempt to dissect wfw3.11 negotiate protocol blob` — conservative handling of an undocumented negotiate blob; useful corroboration of opaque-fallback guidance.
- !21936 — open draft in corpus snapshot — `TCP: Add Client Field to quickly spot client segments` — down-weighted because unmerged. Ronnie Sahlberg challenged treating an unseen handshake as guessed client/server state and suggested unknown state and possibly conversation-level ownership; useful design discussion but not accepted policy.
- !21935 — merged — `GitLab CI: Update WIRESHARK_BASE_DIR in the Windows jobs` — CI/path maintenance; no additive notebook rule.
- !21934 — merged — `Qt: Defer updating data tab layout and scrollbars when not visible` — hidden views may defer expensive layout work; corroborates later accepted UI-performance work already represented elsewhere.
- !21933 — merged release backport — `CMake: Properly remove our artifacts` — CMake list/string quoting correction; no new general rule promoted.
- !21932 — merged — `Qt: Create the hex data source context menu on demand` — lazy GUI allocation/performance improvement; corroborating evidence.
- !21931 — merged master, authored/merged by John Thacker — `PER: Don't add data sources that aren't new` — a subset/aligned tvbuff sharing the same `ds_tvb` is not a distinct Packet Bytes data source. Promoted to `data-source-conventions.md`.
- !21930 — merged — `Qt: Workaround QTabBar O(N^2) bug` — accepted Qt performance workaround/backport family; no cross-project rule promoted.
- !21929 — merged release backport — `Windows: Update GnuTLS to 3.8.10-2` — packaging/dependency maintenance.
- !21928 — merged — `tls: Improve cipher suite expert info` — improves diagnostics for unknown/invalid cipher-suite values; protocol-specific diagnostic refinement.
- !21927 — merged release backport — `Qt: Fix compilation with clang-cl` — portability/build correction; no additive rule.
- !21926 — merged automated update — generated/manuf/services/translations refresh; no substantive human review lesson.
- !21925 — merged automated update — generated/manuf/services/translations refresh; no substantive human review lesson.
- !21924 — merged automated update — generated/manuf/services/translations refresh; no substantive human review lesson.
- !21923 — merged — `packet-http.c/h: reduce padding in structures` — member reordering to reduce per-object padding; optimization, no additive notebook rule.
- !21922 — merged — `wiretap: reduce padding for some structures` — structure-layout memory optimization; no additive rule.
- !21921 — merged master — `SMB: Remove proto_tree_add_string from dissect_create_file_request` — removes an erroneous duplicate/invalid tree addition that triggered Malformed; source change later backported by !21938.
- !21920 — merged — `Windows: Update GnuTLS to 3.8.10-2` — Windows dependency update; no notebook lesson.
- !21919 — merged — `CMake: Properly remove our artifacts` — illustrates that quoted CMake list variables become semicolon-separated strings while unquoted values retain list semantics; build-specific maintenance.
- !21918 — merged — `ORAN FH CUS: Getting ready to decompress modulation compression` — incremental O-RAN decompression preparation; no new cross-project rule.
- !21917 — merged — `Netlogon: add dissection of dsraddresstositenames[ex]w` — protocol coverage expansion; no additive convention.
- !21916 — merged master, authored by Michael Mann and merged by Anders Broman — `Simplify and consolidate some application flavor functionality` — hides the broad flavor getter behind narrower purpose-specific helpers; strongly corroborates existing application-boundary/semantic-API guidance.
- !21915 — merged — `TCP: Fix the Flow Counter statistics` — statistics correction, including ARM macOS behavior; later backport already reviewed as !21989.
- !21914 — merged — `tls: Abbreviated Handshake Using New Session Ticket` — accepted TLS session-ticket behavior/backport family; protocol-specific.
- !21913 — merged — `tls: Abbreviated Handshake Using New Session Ticket` — source-side TLS session resumption correction; no new general rule promoted.
- !21912 — merged — `ORAN FH CUS: Fix a clang uninitialised warning` — initialization/static-analysis cleanup.
- !21911 — merged — `ui: Make the text encoding tables static` — internal tables made file-local/const where appropriate; routine encapsulation cleanup.
- !21910 — merged — `wsutil: Call SHGetKnownFolderPath on Windows` — modernizes Windows known-folder API use; platform maintenance.
- !21909 — merged — `Give detect_bad_proto_tree_add.py execute permission` — tooling metadata fix.
- !21908 — merged — `Qt: Fix compilation with clang-cl` — source-side portability correction later backported by !21927.
- !21907 — merged — `extcap: Do not try to read c:\\etc\\ssh\\ssh_config on Windows` — platform-specific libssh configuration behavior; no additive rule.
- !21906 — merged master, authored/merged by Michael Mann — `decode_as: Abstract reset all call for DCE/RPC` — adds a generic Decode As callback so generic code no longer hard-codes DCE/RPC-specific reset behavior; corroborates existing callback/extension boundary guidance.
- !21905 — merged — `test: don't check the Lrexlib version` — avoids asserting a dependency version that legitimately differs by platform; test portability maintenance.
- !21904 — merged — `headers: Stop shipping broken headers` — do not install public headers whose required include closure is not shipped; corroborates existing public-header validation guidance.
- !21903 — merged — `RDPEAR: Change some IDL not supported by pidl` — generator/input compatibility workaround; no general rule promoted.
- !21902 — merged — `FTDI FT: Dissect missing request types` — adds EEPROM/GPIO request coverage; protocol-specific.
- !21901 — merged master, approved by John Thacker — `wsutil: Use a common struct for SI/IEC prefixes` — accepted successor to !21898: compute `G_N_ELEMENTS` where the static arrays are visible, carry the count explicitly, and test upper limits. Promoted to `c-array-size-conventions.md`.
- !21900 — merged — `HTTP: Fix upgrade info server port` — fixes directional endpoint identity when a server acknowledges an HTTP upgrade; source/backport family with !21897.
- !21899 — merged master, authored/merged by Guy Harris — `Move the compressed file writing to libwsutil` — moves a generic capability to the lowest common utility layer, removes an improper libwritecap→libwiretap dependency/hacks, and enables both callers to share one implementation. Promoted to `library-layering-conventions.md`.
- !21898 — closed/unmerged — `wsutil: check array size that you index (CID1665124)` — down-weighted as implementation evidence. John Thacker explicitly noted that `G_N_ELEMENTS(prefix)` cannot recover the length when `prefix` is a pointer parameter; the accepted successor is !21901.
- !21897 — merged — `HTTP: Fix upgrade info server port` — source-side HTTP upgrade direction fix, later represented again by !21900; no separate rule.
- !21896 — merged master, authored/merged by Michael Mann — `LUA: Move protocol specific functionality to its own file` — isolates protocol-specific Lua helpers so applications such as Stratoshark need not inherit unrelated protocol dependencies; corroborates application/module boundary rules.
- !21895 — merged — `FTDI FT: Register VID/PID dissector preference` — uses existing configurable USB VID/PID registration support; protocol registration feature, no additive rule.
- !21894 — merged master, authored/merged by Michael Mann — `Refactor injecting secrets into pcapng file` — registers protocol-specific secret injection behind a generic interface to remove direct UI→dissector dependencies; strong corroboration of existing generic-frontend callback/registry guidance.
- !21893 — merged — `Qt: Workaround QTabBar O(N^2) bug` — master-side Qt performance workaround; no broad rule promoted.
- !21892 — merged — `ORAN FH CUS: getting config in place for modulation decompression` — incremental protocol feature preparation; no additive convention.
- !21891 — merged release backport — `Qt: Put back the clear call in DataSourceTab` — platform/Qt lifecycle correction; backport evidence only.

## Durable notebook changes

Three additive conventions were promoted from this batch:

1. **Data-source identity (!21931, !21940):** register a derived tvbuff as a new Packet Bytes data source only when it has a distinct underlying `ds_tvb`; a different tvbuff object that is merely a subset/view of the same source should not create a redundant tab. The rule explicitly preserves the converse needed by genuinely transformed payloads. Added to `data-source-conventions.md`.
2. **Array-size metadata survives only if you preserve it (!21898 → !21901):** `G_N_ELEMENTS()` is valid on the actual compile-time array, not on a decayed pointer parameter. Compute the count while the array type is known and carry it explicitly with the pointer. Added to `c-array-size-conventions.md`.
3. **Shared generic capabilities belong in the lowest common layer (!21899):** Guy Harris's merged refactor moves compressed writing into `wsutil` so sibling/higher libraries consume it downward, eliminating both a bad dependency and duplicate implementation. Added to `library-layering-conventions.md`.

High-value corroborating evidence was deliberately not duplicated into new rules: !21916 reinforces narrow semantic application APIs; !21906 and !21894 reinforce generic callbacks/registries instead of protocol dependencies in generic infrastructure; !21896 reinforces isolating protocol-specific helpers for multi-application composition; !21937/!21939 reinforce leaving unknown bytes opaque instead of manufacturing malformed diagnostics. Open !21936 and closed !21898 were down-weighted relative to merged implementations, while their authoritative review comments were retained where useful.
