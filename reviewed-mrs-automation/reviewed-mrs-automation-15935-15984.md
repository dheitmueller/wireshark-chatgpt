# Automated MR review: !15935-!15984

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit: `d6f6e804c1855362fcc1738d4e5c6993b74d82cb`
- Direction: descending MR number (newer to older)
- Reviewed count: **50**
- Selection method: rebuilt the already-reviewed set from the automation ledgers plus `reviewed-mrs.md`; preserved/counts the historical !17571-!17620 batch. The fifty highest-numbered corpus MRs not in that set were !15984 through !15935.

## Exact reviewed MR set

!15984, !15983, !15982, !15981, !15980, !15979, !15978, !15977, !15976, !15975, !15974, !15973, !15972, !15971, !15970, !15969, !15968, !15967, !15966, !15965, !15964, !15963, !15962, !15961, !15960, !15959, !15958, !15957, !15956, !15955, !15954, !15953, !15952, !15951, !15950, !15949, !15948, !15947, !15946, !15945, !15944, !15943, !15942, !15941, !15940, !15939, !15938, !15937, !15936, !15935.

## Review weighting and durable findings

Merged master changes were weighted most heavily; release-branch cherry-picks were treated mainly as corroboration of their originating master fixes, and low-information CI/docs/backport changes were scanned rather than promoted into notebook rules.

- **!15984 / !15983 (merged release backports, Guy Harris):** fix `log3gpp` use of partially uninitialized subsecond digit storage. The parser directly indexes fixed digit positions, so NUL-terminating the short input is not sufficient; missing decimal positions must be initialized to `'0'`. This is useful corroboration for existing initialization/static-analysis guidance, but as release backports it does not justify a duplicate notebook rule.
- **!15980 (merged master, John Thacker):** RPM packaging enables guide generation when prerequisites are available and installs the developer guide in the development package. Useful packaging/build-system context; no new general convention.
- **!15970 (merged release backport):** `capinfos -TM` packet-comment headers were lost because a zero-filled newly allocated buffer was passed to `snprintf` using `strlen(buf)` as its capacity, necessarily yielding zero. The accepted fix tracks allocation capacity independently. This corroborates the existing rule that buffer capacity, current string length, and source length are distinct quantities and must not be interchanged.
- **!15965 (merged master, John Thacker):** WSLua Dumper defaults to pcapng and per-packet encapsulation for pcapng, allowing IDBs to be created on demand while retaining Ethernet as the default for formats with file-level encapsulation. The behavior and API-default change were documented in release notes. This reinforces existing API/default-change documentation practice.
- **!15960 (merged master, Gerald Combs):** fixes scan-build uninitialized-argument warnings by explicitly initializing variables before downstream use. Strong corroboration for treating static-analyzer findings as correctness signals rather than merely suppressing diagnostics.
- **!15955 (merged master, Peter Wu):** centralizes UI wiki links on `wiki.wireshark.org` and uses controlled redirects/consistent protocol URL construction. Project-specific infrastructure guidance only.
- **!15950 (merged master):** MATE user-guide cleanup documenting current grammar/boolean representation. Documentation-only; no new engineering convention.
- **!15945 (merged master, Gerald Combs):** fixes a copy/paste error that sent Intel Logray artifacts to the ARM64 S3 destination. CI-specific correctness fix; no new general rule.
- **!15940 (merged master, Uli Heilmeier):** Falco Follow Stream explicitly declines plugin events and supports only syscall-source records. Reinforces the existing convention that optional UI/analysis operations should decline unsupported record types rather than manufacture a misleading filter.
- **!15935 (merged master, Gerald Combs):** removes unavailable Falco plugin configuration from the macOS CI build. Platform-CI-specific; no new durable convention.

The remaining MRs in the exact set were inspected for state, diff, discussion, and relationship to neighboring/backport changes. None supplied a stronger durable coding, architecture, testing, review, or submission rule than those already represented in the notebook. In particular, duplicated release backports were not promoted over their master-origin evidence.

## Notebook maintenance

No convention file was changed in this run. The useful lessons above corroborate existing initialization/static-analysis, buffer-length/capacity, API-documentation, and graceful-decline guidance, so adding parallel rules would reduce rather than improve the notebook's signal.

## Continuation

On the next run, rebuild the reviewed set from all ledgers again. Subject to that reconstruction and corpus availability, the next descending candidate is **!15934**.
