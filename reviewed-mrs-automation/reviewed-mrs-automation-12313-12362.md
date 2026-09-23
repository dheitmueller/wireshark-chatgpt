# Automated MR review: !12362 through !12313

Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Selection and tracking

Before selecting this run, the already-reviewed set was reconstructed from `reviewed-mrs.md`, the supplemental automation tracker, and the available exact per-run ledgers under `reviewed-mrs-automation/`. The historical !17571-!17620 batch remains preserved and counted. In particular, the preceding authoritative ledger for !12412-!12363 explicitly marked !12362 only as a frontier probe, not as reviewed. Spot checks against the tracking also found no prior review entries for lower members of this batch. The fifty highest-numbered previously unreviewed MRs present in this corpus snapshot therefore happen to be the contiguous set below; that continuity was verified from the tracking rather than assumed from the numbers.

Exact reviewed MR numbers:

`!12362, !12361, !12360, !12359, !12358, !12357, !12356, !12355, !12354, !12353, !12352, !12351, !12350, !12349, !12348, !12347, !12346, !12345, !12344, !12343, !12342, !12341, !12340, !12339, !12338, !12337, !12336, !12335, !12334, !12333, !12332, !12331, !12330, !12329, !12328, !12327, !12326, !12325, !12324, !12323, !12322, !12321, !12320, !12319, !12318, !12317, !12316, !12315, !12314, !12313`

Batch outcome: **45 merged, 5 closed/unmerged** (!12352, !12344, !12326, !12325, !12313). Merged master changes were weighted most heavily. Stable-branch cherry-picks were primarily corroborating evidence, and closed/superseded work was down-weighted except where authoritative review discussion was independently useful.

## Review notes

| MR | Depth | Review result |
|---|---|---|
| !12362 | Deep / promoted | Merged master, authored and merged by John Thacker. `ws_mempbrk_compile()` now clears its entire pattern table before setting requested bytes. The prior implementation worked accidentally for static zero-initialized objects but could misbehave for stack-local storage or recompilation. Promoted to `compiled-state-initialization-conventions.md`; release backports !12363/!12368/!12369 from the preceding batch corroborate it. |
| !12361 | Scanned | Merged release-4.2 CI work generating and publishing the display-filter reference. Useful automation maintenance, no new durable convention. |
| !12360 | Scanned | Merged Debian symbol-version normalization. Packaging-local maintenance. |
| !12359 | Scanned | Merged Debian symbol-list update. Packaging-local maintenance. |
| !12358 | Scanned | Merged removal of a stale lintian spelling override. No architecture lesson. |
| !12357 | Scanned | Merged spelling/filter-name cleanup across dissectors. No new convention. |
| !12356 | Scanned | Merged macOS Sparkle-signature CI work. Packaging automation. |
| !12355 | Deep / corroboration | Merged master, authored by Martin Mathieson. Q.933 `value_string` keys for masked fields are changed from wire-positioned bit values to post-mask logical values. Strongly corroborates existing `field-value-semantics-conventions.md`. |
| !12354 | Deep / corroboration | Merged master, authored by Martin Mathieson. Broadens the same masked-field `value_string` correction across dissectors; corroborates the existing logical-value-after-mask rule. |
| !12353 | Scanned | Merged master macOS setup/dependency maintenance by Michael Tuexen. No distinct convention. |
| !12352 | Closed / down-weighted | Closed/unmerged macOS setup attempt, superseded/reworked by accepted !12353. Not used as implementation precedent. |
| !12351 | Scanned | Merged Sparkle 2.2.2 setup update. Toolchain maintenance. |
| !12350 | Scanned | Merged macOS signing-path CI update. Packaging automation. |
| !12349 | Scanned | Merged macOS path correction. Tooling-local. |
| !12348 | Scanned | Merged master change by João Valverde using GLib's default log handler rather than duplicating handlers for specific domains. Local logging-policy cleanup; no cross-cutting rule added. |
| !12347 | Scanned | Merged Sparkle-signature CI update. Packaging automation. |
| !12346 | Scanned | Merged release-4.2 VMware build-number table update. Protocol data maintenance. |
| !12345 | Discussion/diff scan | Merged master Qt About-dialog release-notes work. Review established that exposing generated release notes in the UI also requires packaging `release-notes.html` on supported platforms. Useful corroboration for packaging/runtime-resource completeness, but no separate rule needed. |
| !12344 | Closed / down-weighted | Closed/unmerged RF4CE dissector attempt with failing pipeline and no accepted review direction. Not used as precedent. |
| !12343 | Scanned | Merged release-4.2 MDB reader-config backport. Intentionally leaves an unsupported packed-BCD field undecoded rather than guessing. Backport evidence only. |
| !12342 | Scanned | Merged MDB Peripheral ID refactoring/helper work. Protocol-local. |
| !12341 | Deep / corroboration | Merged master ZVT cleanup replaces manual packed-BCD conversion/string handling with a registered field plus `proto_tree_add_item()` and BCD encoding flags. Strongly corroborates `field-decoding-api-conventions.md`. |
| !12340 | Scanned | Merged MDB dissector expansion. Protocol-local implementation. |
| !12339 | Scanned | Merged release-4.2 MDB max/min price backport. No additional review lesson. |
| !12338 | Deep / corroboration | Merged master NFS masked-value correction by Martin Mathieson, with a synthetic/Scapy validation capture. Corroborates the existing field-value semantics rule and the value of focused generated regressions. |
| !12337 | Scanned | Merged VMware build-number table update. Data maintenance. |
| !12336 | Scanned | Merged release-4.2 EasyMesh 5.0 specification backport. Primary compatibility discussion is on master !12314 below. |
| !12335 | Scanned | Merged release-4.2 iperf3 Info-column backport. No distinct rule. |
| !12334 | Deep / promoted with !12313 | Merged master, Gerald Combs. Extends the GLib-type conversion tooling and migrates applicable 64-bit constant macros to standard `INT64_C()` / `UINT64_C()`. This is the accepted implementation follow-up to the dependency discussion in closed !12313 and is primary accepted evidence for `dependency-migration-conventions.md`. |
| !12333 | Scanned | Merged IEEE 1609.2 ASN.1/CAMP update. Specification/generated maintenance. |
| !12332 | Deep / corroboration | Merged master AFP correctness fix, approved/merged by John Thacker. A helper had overloaded NULL `packet_info *` to mean "do not update the Info column", but later needed `pinfo->pool`; the accepted fix always passes valid packet context and adds an explicit `add_info` flag. Strongly corroborates the existing `dissector-pipeline-conventions.md` packet-context rule; that file was updated with this evidence. |
| !12331 | Scanned / corroboration | Merged master TS32.423 fix frees `GMatchInfo`, simplifies regex handling, and also fixes an addressless-packet crash discovered during the work. Reinforces cleanup/defensive parsing without a new rule. |
| !12330 | Scanned | Merged release-4.2 Windows CI update to Qt 6.5.3. Toolchain maintenance. |
| !12329 | Discussion scan | Merged Qt-LTS documentation update. Anders Broman subsequently asked that obsolete parallel MRs be checked and cancelled; useful project hygiene but not a durable coding convention. |
| !12328 | Scanned | Merged master Windows CI update to Qt 6.5.3. Toolchain maintenance. |
| !12327 | Deep / corroboration | Merged master, authored and merged by João Valverde. Fixes a display-filter warning use-after-free by materializing needed text before freeing the associated error state and adds a regression test for the warning. Strong lifetime/test evidence; existing notebook guidance already covers the general rule. |
| !12326 | Closed / down-weighted | Closed TS32.423 regex-memory optimization; Anders Broman concluded it was no longer needed. Superseded discussion only. |
| !12325 | Closed / down-weighted | Earlier closed TS32.423 memory-utilization attempt; likewise marked no longer needed. Not precedent. |
| !12324 | Scanned | Merged WBXML recursion-level correction: nested tags had incremented depth twice. Local correctness fix. |
| !12323 | Scanned | Merged release-4.2 MDB cashless-setup backport. No new convention. |
| !12322 | Scanned | Merged master MDB reader-config dissection. Deliberately postpones a packed-BCD field until its encoding can be handled correctly. Good restraint, but protocol-local. |
| !12321 | Deep / promoted | Merged master, authored by John Thacker and approved/merged by Anders Broman. IPv6 parent-item length finalization is moved into `TRY`/`FINALLY` so exceptions, truncated captures, and incomplete-fragment paths cannot bypass a required parent-tree invariant. Added to `exception-boundary-conventions.md`. |
| !12320 | Scanned | Merged release-3.6 backport of the Qt protocol-byte highlighting fix. |
| !12319 | Scanned | Merged release-4.0 backport of the same Qt highlighting fix. |
| !12318 | Scanned | Merged release-4.2 MDB filter-abbreviation simplification. Filter names are organized around reusable components rather than unnecessarily embedding one command context. Backport/local naming evidence. |
| !12317 | Discussion/diff scan | Merged master MDB max/min prices. Martin Mathieson caught a missing quote and an offset-expression simplification; both were addressed before approval. No broader rule. |
| !12316 | Scanned | Merged master iperf3 Info-column correction to show the protocol's transmitted timestamp rather than local relative time. Protocol presentation fix. |
| !12315 | Scanned | Merged master AWDL fix gives two incompatible registered field types distinct abbreviations instead of sharing `awdl.serviceparams.values`. Straightforward field-registration correctness. |
| !12314 | Deep / promoted | Merged master EasyMesh 5.0 update. Alexis La Goutte explicitly questioned deleting older conflicting TLV definitions because existing pcaps might depend on them. Albert Chuang checked released 3.0/4.0/5.0 specs and project history and showed the old meanings came from a pre-release draft and had been superseded before later specifications reassigned those values. Promoted to `protocol-version-compatibility-conventions.md`: verify authoritative released specs/history and explicitly consider old-capture compatibility when numeric values are reassigned. |
| !12313 | Discussion-focused, closed / authoritative corroboration | Closed GLib-header cleanup proposal. Guy Harris provided high-authority standards analysis that `UINT64_C()` is the rough standard replacement for `G_GUINT64_CONSTANT()`, while noting the exact `uint_least64_t` contract. Jaap Keuter recommended keeping `glib.h` localized in remaining users until conversion; Gerald Combs moved the conversion into accepted !12334. The closed patch itself is down-weighted, but Guy's analysis plus accepted !12334 supports `dependency-migration-conventions.md`. |

## Durable notebook updates

- Added `compiled-state-initialization-conventions.md` from merged master !12362, with !12363/!12368/!12369 as release-branch corroboration.
- Updated `dissector-pipeline-conventions.md` with merged master !12332 as independent evidence that optional presentation policy should not be encoded by nulling required packet context.
- Updated `exception-boundary-conventions.md` from merged master !12321: mandatory parent-dissection postconditions that must survive child exceptions/early exits belong in guaranteed finalization.
- Added `dependency-migration-conventions.md` from accepted master !12334 plus Guy Harris's high-authority standards discussion in closed !12313: migrate remaining use sites before removing dependency includes, and verify the precise type contract of standard portability replacements.
- Added `protocol-version-compatibility-conventions.md` from merged master !12314: when specification revisions reuse numeric values, verify released-spec/history provenance and explicitly evaluate compatibility with existing captures before deleting or preserving the old interpretation.
- !12355/!12354/!12338 corroborate the existing masked-field logical-value guidance; !12341 corroborates the standard field-decoding API guidance; !12327 corroborates lifetime-aware regression testing. These were not duplicated into new rules.

## Frontier check

!12312 (`NTP: remove wrong value from peer event code vs`) exists in this same corpus snapshot and is merged on release-4.2. It was inspected only as the frontier check and is **not** counted as reviewed in this run. The corpus therefore has not run out of previously unreviewed MRs, and no scraper restart is needed.