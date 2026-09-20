# Wireshark MR automation review: !15648 through !15599

Corpus commit: `130eea3c8a60902c0fc98c9e1130e73bafc6f815`

## Selection

Before selecting this batch, the already-reviewed set was rebuilt from the available `reviewed-mrs-automation/` per-run ledgers and `reviewed-mrs.md`, with the historical !17571-!17620 batch explicitly preserved and counted. The immediately preceding authoritative ledger covers !15698 through !15649. Repository-wide review-tracking searches found no already-reviewed candidate below !15649 that would displace an MR in this batch, and the corpus contains every MR from !15648 through !15599. Therefore the fifty highest-numbered corpus MRs not already in the reviewed set are exactly !15648 through !15599 inclusive. Selection was by exact reviewed-set subtraction; no numeric range was assumed reviewed merely because some members appeared in a ledger.

Weighting: merged master changes and substantive maintainer review carry the most weight; maintained-branch backports corroborate master behavior; closed/superseded work is recorded but deliberately down-weighted. In particular, !15647 and !15609 were closed without merge. Guy Harris-authored-and-merged !15610 receives exceptional authority weight.

## Exact reviewed set

| MR | Outcome / review note |
|---|---|
| !15648 | Merged. NTLMSSP adds the distinct anonymous-authentication session-key path instead of assuming NT/LM response material exists; focused crypto/session-state correctness fix. |
| !15647 | Closed, unmerged. MikroTik RADIUS dictionary attribute-name update; conflicted/abandoned state gives it little convention weight. |
| !15646 | Merged. LINK16 Message/NPG display-string update; protocol presentation maintenance, no new cross-cutting rule. |
| !15645 | **Deep, merged master; authored/merged by Gerald Combs.** Adds display-filter translation API plus pcap/Falco translators. John Thacker identified semantic non-equivalences involving repeated fields, field references, field-to-field comparisons, and order-sensitive BPF VLAN/MPLS constructs. Promoted to `filter-translation-conventions.md`: refuse translations whose semantics cannot be preserved. |
| !15644 | **Deep, merged.** John Thacker moves Clang+Code Checks to a larger SaaS runner because clang-tidy/recursion analysis was timing out; Gerald Combs approved and discussed enabling more analyzers. John notes CheckAPI can now run independently and code checks can be split if needed. Corroborates CI capacity/decomposition guidance. |
| !15643 | Merged. macOS setup updates Ninja to 1.12.1; dependency maintenance only. |
| !15642 | **Deep, merged master; John Thacker approval.** Fixes BTLE memory corruption caused by storing `connection_parameter_info` in a wmem tree whose consumers expect another value type. Promoted to `keyed-state-container-conventions.md`. |
| !15641 | Merged. Martin Mathieson flags UA3G `value_string` values that do not fit their masks; useful checker evidence, but the poorly documented/possibly dormant protocol limits broader inference. |
| !15640 | **Deep, merged master; authored/merged by John Thacker.** Makes CheckAPI explicitly depend on the source/generated files it inspects so the target works from a clean source tree. Promoted to `checker-target-conventions.md`. |
| !15639 | Merged; authored/merged by Martin Mathieson. Fixes a copy/paste bug in `check_typed_item_calls.py` and improves Zigbee command labels. Corroborates treating project checkers as code that itself needs semantic review. |
| !15638 | Merged maintained-branch RTP Player crash fix. Audio-stream teardown can synchronously invoke `playFinished`; cleanup ordering is changed and `marker_stream_` is guarded. Corroborates lifecycle/re-entrant callback safety. |
| !15637 | Merged release preparation for 3.6.23; release mechanics only. |
| !15636 | Merged release preparation for 4.0.15; release mechanics only. |
| !15635 | Merged release preparation for 4.2.5; release mechanics only. |
| !15634 | Merged RTP Player crash lineage/backport; same teardown/callback lifetime correction as !15638, counted as corroboration rather than independent architectural evidence. |
| !15633 | Merged RTP Player crash lineage/source fix; same lifecycle issue, weighted once across the series. |
| !15632 | Merged macOS CI Lua restoration/backport; platform build maintenance. |
| !15631 | Merged macOS CI Lua restoration variant/backport; corroborating platform maintenance. |
| !15630 | Merged openSUSE CI image update to 15.5; build-environment maintenance. |
| !15629 | Merged GitLab CI separator/path cleanup; localized CI syntax/portability fix. |
| !15628 | Merged openSUSE CI image update lineage; maintained-branch corroboration. |
| !15627 | Merged CI variable quoting fix; ordinary shell/CI robustness. |
| !15626 | Merged RTP Player crash lineage/backport; corroborates the !15633/!15638 teardown ordering fix. |
| !15625 | Merged macOS CI Lua restoration lineage; ordinary CI platform maintenance. |
| !15624 | Merged spelling cleanup; no durable engineering convention. |
| !15623 | **Reviewed, merged with sample capture.** Zabbix recognizes the pre-7.0 `ZBX_NOTSUPPORTED\0Error message` passive-agent form, preserves the marker in `zabbix.data`, and exposes the message separately in `zabbix.error`. Good protocol-version/sample-capture practice; no new notebook rule beyond existing semantic-field/testing guidance. |
| !15622 | **Reviewed, merged master; John Thacker merged.** Converts compile-time-constant local arrays/objects to `static const`, eliminating repeated initialization code (reported 3.8 KiB text reduction for 1.8 KiB read-only data). Useful performance/style evidence but not promoted as a universal rule. |
| !15621 | Merged IEEE 802.11 HE PHY BSS membership selector addition; focused protocol coverage update. |
| !15620 | Merged MinGW/Qt 6.7 packaging adaptation for renamed Windows style plugin; versioned dependency maintenance. |
| !15619 | Merged CQL cleanup using `val_to_str_const` where no formatting substitution is required; helper-API cleanup already covered by existing conventions. |
| !15618 | Merged maintained-branch WSLua duplicate-registration crash fix; backport of !15611 protected-call behavior. |
| !15617 | **Deep, merged maintained branch; John Thacker.** WSLua invokes `Proto_commit` through `lua_pcall` so duplicate field/expert registration becomes a reported Lua failure instead of reaching `wslua_panic` and aborting Wireshark. Promoted with the master change to `scripting-error-boundary-conventions.md`. |
| !15616 | Merged maintained-branch WSLua duplicate-registration crash fix; corroborates !15611. |
| !15615 | Merged MinGW/Qt 6.7 packaging adaptation lineage; maintained-branch/platform corroboration. |
| !15614 | Merged documentation CI artifact fix; localized pipeline maintenance. |
| !15613 | Merged documentation CI dependency fix (`ruby-dev`); localized job dependency maintenance, consistent with keeping job-specific requirements close to the consuming job. |
| !15612 | **Reviewed, merged master; Gerald Combs authored, Anders Broman merged.** Converts the column API to C99 fixed-width/standard types; API modernization and type-domain cleanup, no separate rule promoted in this batch. |
| !15611 | **Deep, merged master; authored/merged by John Thacker.** Source WSLua duplicate-registration crash fix. Protected Lua call plus object-specific diagnostics promoted to `scripting-error-boundary-conventions.md`. |
| !15610 | **Deep, merged master; authored/approved/merged by Guy Harris.** Correctly interprets SMB2 Negotiate bytes according to pre-3.1.0 versus 3.1.0+ semantics. Older reserved/MBZ fields are diagnosed if nonzero and forced to zero internally so they cannot trigger newer negotiate-context parsing. Promoted to `protocol-version-field-conventions.md` with exceptional authority weight. |
| !15609 | **Discussion-focused, closed/unmerged.** Proposed splitting the ~1500-line BTLE dissector. Review found an uninitialized variable, field-label issues, and—most importantly—John Thacker required the earlier mixed-value-type wmem-tree regression to be fixed before further refactoring. Down-weighted as unmerged, but corroborates the !15642 state-container rule and fix-correctness-before-refactor workflow. |
| !15608 | Merged Qt I/O Graph UAT-header resize-to-contents context menu; focused UI usability feature. |
| !15607 | **Deep, merged release-4.2; John Thacker authored, Anders Broman merged.** Reverts `pushinteger` changes because supported Lua 5.1/5.2 gain little and 32-bit casts can produce incorrect unsigned values. Strong stable-branch compatibility evidence: validate supported dependency versions and architectures before backporting representation/API changes. Existing stable-branch guidance is sufficient, so no duplicate notebook rule was added. |
| !15606 | Merged release-note update for graph improvements; documentation maintenance. |
| !15605 | Merged automatic assigned-data/translation update; low convention weight. |
| !15604 | Merged automatic assigned-data/translation update; low convention weight. |
| !15603 | Merged automatic assigned-data/translation update; low convention weight. |
| !15602 | Merged automatic assigned-data/translation update; low convention weight. |
| !15601 | **Deep, merged master; authored/merged by John Thacker.** Fixes I/O Graph crashes by deferring retap with a zero-delay single-shot timer, refusing nested retaps, and queueing work until the active tap completes. Strong corroboration of existing reentrancy/event-loop guidance; no duplicate rule added. |
| !15600 | **Reviewed, merged release-4.2; John Thacker.** Tracks heuristic dissector lists created from Lua so reload can deregister the correct resource; also documents that the API currently cannot return the created object cleanly and therefore has awkward ownership. Corroborates existing reload/registration-lifecycle guidance. |
| !15599 | Merged documentation synchronization for static initial values changed by earlier commits; documentation consistency only. |

## Durable notebook changes from this run

- `filter-translation-conventions.md` — translators must preserve target-language semantics, not merely rewrite syntax; unsupported semantic forms must be refused.
- `protocol-version-field-conventions.md` — version-gate field meaning and downstream parsing; invalid old-version reserved bytes must not activate new-version substructures.
- `checker-target-conventions.md` — standalone checker targets must declare generated-source dependencies and work from a clean build state.
- `keyed-state-container-conventions.md` — treat untyped keyed containers as having a logical value type; use separate containers or explicit discriminators for unrelated record types.
- `scripting-error-boundary-conventions.md` — script-triggerable registration/commit operations that can raise must cross a protected runtime boundary when failure should be reported rather than abort Wireshark.

The exact reviewed count for this run is 50.