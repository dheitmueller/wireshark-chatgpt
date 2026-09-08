# Automated Wireshark MR review: !26298–!26347

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Notebook base before this run: `1dfe2f126ed99d5d09c2f91de212b9bb4405838d`

Review scope: exactly 50 previously unreviewed merge requests, !26298 through !26347 inclusive. Merged MRs and accepted stable-branch backports were weighted more heavily than open, draft, closed, or unresolved proposals. Maintainer comments were weighted by demonstrated project/domain authority, with particular weight given to Gerald Combs, Guy Harris, Anders Broman, Stig Bjørlykke, Michael Mann, and other established maintainers appearing in this window.

## Durable findings promoted

- !26326: Windows runtime deployment is a versioned toolchain prerequisite; the installed VC++ Redistributable must satisfy the newest toolset used by any component. Gerald Combs authored/merged the change; Guy Harris and Michael Mann supplied high-authority deployment/installer discussion.
- !26318: prevent dfilter macro cycles through nested expansion semantics, not merely an arbitrary recursion-depth limit.
- !26305 plus !26307/!26308: compiler register/cache reuse is valid only when semantic qualifiers such as layer ranges/raw forms are part of the identity.
- !26306: reject invalid macro argument references at definition time, including `$0` and integer overflow.
- !26314: verify AI-generated documentation against actual implementation semantics.
- !26329 plus !26332/!26333 and !26331 plus !26334/!26335: prove intermediate-expression range, not merely final-result range; widen before the overflowing operation.
- !26300: reject zero divisors/scaling invariants at the metadata boundary and retain a defensive check at use.
- !26327: hash/equality helpers must match the actual key representation and alignment; raw packet bytes are not automatically suitable for typed integer hashes.
- !26328: make container allocator/lifetime match its owning wmem state.
- !26330/!26336: prefer lookup/iteration to temporary hash key/value snapshots, and use one canonical NULL-safe destructor after explicit ownership transfer.
- !26302: distinguish interface refresh from expensive extcap invalidation; reuse freshly enumerated extcaps unless the trigger can make them stale. Gerald Combs approved and merged the change.
- !26325: reproducer captures belong on issues/MRs, while maintained test captures belong in the proper test-data structure, not the repository root.
- !26304: keep contribution-policy trailer spelling, CI enforcement, and remediation text synchronized.
- !26313: Jaap Keuter reinforced use of `Fixes #NNNN` for changes intended to close an issue; the O-RAN implementation itself remains open/unresolved and was not promoted as accepted architecture.

## Exact MR accounting

| MR | State at corpus snapshot | Weight | Review disposition |
|---|---|---|---|
| !26298 | merged | high | Toshiba stable backport; validates line length before fixed-offset LEN access; corroborates bounded-input rules. |
| !26299 | merged | high | Same Toshiba fix for release-4.4; corroborating backport. |
| !26300 | merged | very high | NetXRay rejects zero `ticks_per_sec`; Anders Broman approval; promoted arithmetic invariant rule. |
| !26301 | merged | high | Bluetooth LE display labels corrected while IDs/masks remain stable; no new generic convention. |
| !26302 | merged | very high | InterfaceListManager avoids unnecessary extcap rediscovery; Gerald Combs approval/merge; promoted invalidation rule. |
| !26303 | merged | high | ERF writer refuses truncation arithmetic that would underflow; corroborates output-bound arithmetic. |
| !26304 | merged | very high | CI updated from `AI-Assisted` to `Assisted-by`; Gerald Combs merge; promoted policy/enforcement synchronization. |
| !26305 | merged | very high | Dfilter range-limited values cannot be reused as semantically different field forms; promoted cache-identity rule. |
| !26306 | merged | very high | Dfilter validates one-based macro argument numbers and integer range at definition time; promoted. |
| !26307 | merged | high | Release-4.6 backport of !26305; corroborating evidence. |
| !26308 | merged | high | Release-4.4 backport of !26305; corroborating evidence. |
| !26309 | merged | medium | Dissector warning cleanup; reviewed, no durable generic rule extracted. |
| !26310 | merged | high | Lemon parser pointer cleared after free; corroborates lifecycle hygiene. |
| !26311 | merged | very high | IEEE 1722 2025 updates accepted/merged by Anders Broman; protocol-specific, no generic rule extracted. |
| !26312 | merged | very high | Stig Bjørlykke removes redundant `AT Command` Info text; supports concise columns but not promoted as separate rule. |
| !26313 | opened | provisional | O-RAN fragmentation proposal remains unresolved; Martin Mathieson warns incomplete fragments may not be complete sections. Only independent submission guidance promoted. |
| !26314 | merged | very high | Corrects inaccurate AI-generated dfilter Doxygen semantics; promoted documentation verification rule. |
| !26315 | merged | very high | Default-profile path correction accepted by Anders Broman; profile-specific correctness, no new generic rule. |
| !26316 | merged | very high | Gerald Combs extcap interface-name conversion consistency, merged by Anders Broman; corroborates central conversion helpers. |
| !26317 | merged | high | Qt validators parented correctly to prevent leak; corroborates QObject ownership conventions. |
| !26318 | merged | very high | Dfilter macro nested-rescan semantics prevent cycles; promoted. |
| !26319 | merged | very high | Gerald Combs extcap bookmark feature, merged by Anders Broman; UI feature, no generic rule extracted. |
| !26320 | merged | very high | Gerald Combs macOS GLib/rpath maintenance; reviewed as build-specific maintenance. |
| !26321 | closed draft | low | Large UDX enhancement abandoned/closed with conflicts; not used as architectural evidence. |
| !26322 | merged | very high | Stig Bjørlykke DECT NR context validation from Coverity; corroborates nullable-context checks. |
| !26323 | opened draft | provisional | HyperDHT draft with detailed local validation/AI disclosure; not treated as accepted architecture. |
| !26324 | merged | very high | Extcap control-pipe graceful-shutdown documentation, merged by Gerald Combs; corroborates existing extcap runtime rules. |
| !26325 | merged | very high | Removes stray root-level capture; Gerald Combs merge; promoted repository/test-artifact hygiene. |
| !26326 | merged | extremely high | Gerald Combs Windows runtime installer change with Guy Harris/Michael Mann discussion; promoted. |
| !26327 | merged | very high | WireGuard replaces integer hash on potentially unaligned packet bytes; promoted key-representation rule. |
| !26328 | merged | very high | TWAMP switches file-scope session container from GSList to wmem list; promoted lifetime-domain rule. |
| !26329 | merged | very high | 7-bit byte-count intermediate overflow fixed with wider type; promoted. |
| !26330 | merged | very high | Extcap error-path ownership simplified; direct lookup replaces temporary key list; promoted container API rules. |
| !26331 | merged | very high | Sharkd resampling intermediate widened; promoted with !26329. |
| !26332 | merged | high | Release-4.6 backport of !26329; corroborating evidence. |
| !26333 | merged | high | Release-4.4 backport of !26329; corroborating evidence. |
| !26334 | merged | high | Release-4.6 backport of !26331; corroborating evidence. |
| !26335 | merged | high | Release-4.4 backport of !26331; corroborating evidence. |
| !26336 | merged | very high | Extcap uses `GHashTableIter` rather than allocated key/value lists; promoted with !26330. |
| !26337 | merged | very high | RTPS DATA_BATCH inner loop uses its own minimum-length variable; corroborates local nested bounds. |
| !26338 | merged | high | O-RAN small protocol updates by Martin Mathieson; protocol-specific. |
| !26339 | merged | high | TLS GREASE handling added for signature algorithms; protocol-specific extensibility fix. |
| !26340 | merged | very high | Frame-comment tree construction placed under dissector exception handling; Anders Broman merge; corroborates exception-boundary rules. |
| !26341 | merged | high | Release-4.6 backport of !26337; corroborating evidence. |
| !26342 | merged | high | Release-4.4 backport of !26337; corroborating evidence. |
| !26343 | merged | high | Release-4.6 backport of TLS GREASE change; corroborating evidence. |
| !26344 | merged | very high | Clang Analyzer dead-store cleanup, merged by Anders Broman; tooling cleanup, no generic rule extracted. |
| !26345 | merged | high | Automated generated-data update for release-4.4; no new generic rule. |
| !26346 | merged | high | Automated generated-data update for release-4.6; no new generic rule. |
| !26347 | merged | high | Automated generated-data update for master; no new generic rule. |

## Notebook files changed in this run

- `arithmetic-safety-conventions.md`
- `dfilter-conventions.md`
- `container-api-conventions.md`
- `extcap-discovery-conventions.md`
- `submission-conventions.md`
- `windows-runtime-conventions.md`
- this ledger file

The open/draft MRs in this range were recorded as reviewed so they will not be revisited by the sequential ledger, but unresolved implementation proposals were deliberately given less evidentiary weight than merged master changes and stable backports.