# Automated MR review ledger: !24909–!24958

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Notebook base before this run: `4f5a48d043374d9f72935324aa152145f58a098e`

Selection method: rebuilt the already-reviewed set from `reviewed-mrs.md`, the generic supplemental ledger in `reviewed-mrs-automation/reviewed-mrs-automation.md`, and all available per-run ledgers under `reviewed-mrs-automation/`; then selected the 50 highest-numbered corpus MRs absent from that explicit set. Numeric ranges were not treated as reviewed merely because a neighboring MR or range-like filename existed. The previously reviewed historical batch !17571–!17620 remains part of the reviewed set and is preserved/counts as 50 earlier reviews.

This run reviewed exactly 50 MRs, in descending order: !24958 through !24909 inclusive.

| MR | Depth | State | Review result |
| --- | --- | --- | --- |
| !24958 | Scan | Merged | CI rclone environment-variable correction. `RCLONE_CONFIG` has tool-defined semantics, so Wireshark moved its own option bundle to a non-conflicting name; useful CI hygiene, but too tool-specific for a separate notebook rule. |
| !24957 | Deep | Merged | Consolidates duplicated checking-script source parsing into a common parser. Good repository-tooling direction and consistent with later checker single-source-of-truth guidance; no new convention promoted. |
| !24956 | Scan | Merged | Windows rclone configuration follow-up reverting to explicit/manual setup after the preceding approach proved unsuitable. Useful evidence to weight the final accepted CI sequence rather than any intermediate patch. |
| !24955 | Scan | Merged | Adds a Qt WCA banner slide. Review touched PNG compression/size; narrow UI/resource change, no durable convention. |
| !24954 | Scan | Merged | Stable-branch Windows rclone package-job backport; corroborates the CI migration series but adds no independent architectural guidance. |
| !24953 | Deep | Merged | Fixes PowerShell expansion of rclone configuration arguments by explicitly splitting/expanding the environment variable. Reinforces shell-specific argument-boundary care; no separate rule promoted. |
| !24952 | Deep | Merged | Adds `weekly-updates.py` to orchestrate registry/author-list generators and adjusts subordinate scripts to a common automation contract. John Thacker noted automated sources can have branch-specific retrieval failures; useful tooling evidence, but existing generated-data guidance is sufficient. |
| !24951 | Scan | Merged | Moves rclone configuration into a CI variable. Intermediate step in the accepted rclone series; final-state MRs are weighted more heavily than this individual mechanical change. |
| !24950 | Deep | Merged | Replaces local/raw time-unit conversion definitions with shared `wsutil/nstime.h` constants. Combined with !24943/!24946 and epoch cleanups into semantic constant guidance in `architecture.md`. |
| !24949 | Scan | Merged | Automatic registry/manuf/services/translation update; generated-data refresh with no novel review signal. |
| !24948 | Scan | Merged | Automatic registry/manuf/services/translation update; no new convention. |
| !24947 | Scan | Merged | Automatic registry/manuf/services/translation update; no new convention. |
| !24946 | Deep | Merged | Guy Harris-authored/merged correction removes conversion constants whose names expressed the inverse unit relationship. Promoted: shared conversion constants must encode direction correctly; mathematically correct values under reversed names are API hazards. |
| !24945 | Deep | Merged | Nested TLS follow-up makes `tls_set_appdata_dissector` and `ssl_starttls` use protocol depth, completing earlier nested-TLS context fixes. Approved/merged by John Thacker; strong corroboration of existing nested-context guidance, not duplicated. |
| !24944 | Scan | Merged | Makes CI rclone print copied files. Improves observability of packaging/publishing, but existing CI diagnostic guidance covers the principle. |
| !24943 | Deep | Merged | Adds correctly named shared nstime conversion constants and inlines simple helpers. Corroborates !24946 and the shared-unit/constant rule promoted to `architecture.md`. |
| !24942 | Deep | Merged | DNS SVCB/HTTPS malformed parameter hardening: validates fixed/header and key-specific lengths, bounds inner parsing, reports Expert Info, and resynchronizes at the declared parameter boundary. Promoted to `parser-boundary-conventions.md`. |
| !24941 | Deep | Merged | John Thacker adds compiler `-Wmissing-variable-declarations`; discussion favors compiler enforcement over duplicating the language rule in a source checker, with narrow architectural exceptions. Promoted to `build-conventions.md`. |
| !24940 | Deep | Merged | Adds C-array Follow Stream output to TShark and aligns YAML indexing with GUI output. Good CLI/GUI output-contract parity, but no broad convention beyond existing shared-feature behavior. |
| !24939 | Scan | Merged | Another rclone command correction in CI; intermediate operational fix, no new durable rule. |
| !24938 | Deep | Merged | John Thacker marks translation-unit-local globals `static` based on the new compiler warning; approved/merged by Guy Harris. Combined with !24941 as strong evidence for explicit linkage intent. |
| !24937 | Scan | Merged | Corrects `wmem_strbuf` append documentation to state that helpers append C-style escape sequences. Documentation accuracy fix, no distinct architecture rule. |
| !24936 | Deep | Merged | X.509 export filenames switch from certificate serial alone to SHA-256 of the complete certificate because serial is unique only with issuer context. Strong identity-design example, but narrow enough not to add a new general rule. |
| !24935 | Scan | Merged | Stable-branch backport of WSLua `FileHandler.supported_comment_types` iterator fix; corroborates master fix !24931. |
| !24934 | Scan | Merged | Debian tooling prefers Lua 5.5 where available. Dependency-version maintenance, no durable convention beyond existing capability/discovery guidance. |
| !24933 | Deep | Merged | WSLua debugger reload cleanup restores debugger state before `wslua_init`, removes delayed bookkeeping, and avoids duplicate failure reporting when debugger handling already consumed the error. Good state-transition cleanup; existing state-modeling guidance is sufficient. |
| !24932 | Scan | Merged | Moves dissector-data generator scripts into a dedicated tools subdirectory. Repository organization improvement; no new coding rule. |
| !24931 | Scan | Merged | Fixes WSLua `FileHandler.supported_comment_types` loop iterators on master. Straight correctness fix, with !24935 as stable backport. |
| !24930 | Deep | Merged | John Thacker replaces ad-hoc CPU detection with optional `cpuinfo`, while retaining native fallback paths if the library is unused or cannot supply data. Guy Harris explicitly examined unsupported-platform behavior and upstreaming platform support. Useful portability evidence, but existing dependency/fallback guidance covers it. |
| !24929 | Scan | Merged | CI rclone command fix; operational follow-up with no independent durable convention. |
| !24928 | Deep | Merged | ETL reader replaces local FILETIME epoch arithmetic with the authoritative `wsutil/epochs.h` definition and cleans related wording. Corroborates shared semantic constants. |
| !24927 | Scan | Merged | Switches Windows package jobs to rclone. Part of a multi-MR migration whose later corrections establish the final accepted configuration. |
| !24926 | Deep | Merged | Observer reader replaces its private 2000-epoch definition with the shared `wsutil/epochs.h` value. Corroborates single authoritative semantic constants. |
| !24925 | Deep | Merged | John Thacker adds a packet-range iterator that owns range initialization and selection advancement, abstracting callers from `packet_range_process_packet` mechanics. Good API encapsulation; no new convention beyond existing owner/abstraction guidance. |
| !24924 | Scan | Merged | Adds CIP Process Device Profile object dissection. Substantive protocol coverage but little general coding/review signal in this corpus entry. |
| !24923 | Scan | Merged | Export Objects progress UI is restored when a retap restarts. UI lifecycle correctness; related teardown lifetime rule is captured more strongly by !24913/!24915/!24916. |
| !24922 | Deep | Merged | John Thacker replaces unsigned subtraction-as-condition with `tvb_reported_length_remaining()`, avoiding underflow semantics and expressing the packet-boundary question directly. Strong corroboration of existing unsigned/arithmetic-domain guidance. |
| !24921 | Deep | Merged | PROFINET secure RTC detection is tightened from a length-only discriminator to checks incorporating actual security-frame structure. Good evidence that dissector classification should use semantic discriminators rather than incidental length alone; no separate rule added because heuristic/registration guidance already covers this class. |
| !24920 | Deep | Merged | Replaces repeated Unix-to-Y2K epoch arithmetic with the shared epoch constant. Corroborates !24926/!24928 and the semantic-constant promotion. |
| !24919 | Deep | Merged | Updates BGP SR Policy path-attribute support for RFC 9830/9831. Standards-driven protocol expansion; accepted implementation but no novel cross-cutting convention extracted. |
| !24918 | Deep | Merged | John Thacker deregisters dynamically registered MIB protocol/fields at shutdown and moves associated key storage to appropriate long-lived allocation. Important lifecycle work, but interaction with changing hfids makes runtime re-registration subtler; retained as evidence rather than overgeneralizing a new rule. |
| !24917 | Deep | Merged | Large AI-assisted Doxygen conversion across Wireshark headers; accepted after rebase/CI. Documentation modernization with no single new architecture/coding convention beyond existing documentation expectations. |
| !24916 | Scan | Merged | Stable-branch counterpart of Export Objects post-retap teardown fix; corroborates !24913. |
| !24915 | Scan | Merged | Stable-branch counterpart of Export Objects post-retap teardown fix; corroborates !24913. |
| !24914 | Deep | Merged | Gerald Combs optimizes `check_apis.py` TRY/CATCH checking by first intersecting parsed file words with marker tokens before running expensive regex logic. Useful checker-performance pattern, but too implementation-specific for a separate rule. |
| !24913 | Deep | Merged | John Thacker moves Export Objects cleanup to `endRetapPackets()` because the dialog can close before retapping completes, otherwise leaving callbacks to access freed memory. Promoted to `memory-lifetime-conventions.md`, with !24915/!24916 corroboration. |
| !24912 | Scan | Merged | Bumps Npcap to 1.88. Dependency maintenance only. |
| !24911 | Scan | Merged | Adds UBX-MON-RF dissection. Protocol feature addition with no new cross-cutting convention extracted. |
| !24910 | Scan | Merged | Lua debugger About information shows `LUA_RELEASE`, making behavior-affecting runtime version visible. Useful diagnostic affordance but narrow. |
| !24909 | Scan | Merged | Stable-branch welcome-page help infrastructure backport. Documentation/UI support change; no new convention. |

## Durable notebook changes from this run

- `build-conventions.md`: use compiler diagnostics to enforce linkage intent where possible; external globals require an explicit declaration/contract, otherwise translation-unit-local objects should be `static` (!24941, corroborated by !24938 and Guy Harris approval/merge).
- `parser-boundary-conventions.md`: malformed length-delimited child elements should stay bounded to their declared extent and, when outer framing remains trustworthy, resynchronize at that declared boundary so subsequent elements remain parseable (!24942).
- `architecture.md`: semantic conversion constants must have names that encode conversion direction correctly, and shared authoritative time/epoch constants should replace locally repeated literals/definitions (!24946, !24943, !24950, !24920, !24926, !24928). Guy Harris's !24946 carries especially high weight.
- `memory-lifetime-conventions.md`: teardown for asynchronous/retap work belongs at the framework completion callback, not after the initiating call returns (!24913 with stable-branch corroboration !24915/!24916).

No update to `reviewed-mrs.md` was required for correctness; this exact run ledger is the authoritative audit record for these 50 reviews.