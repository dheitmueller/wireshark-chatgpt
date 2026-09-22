# Automated MR review ledger: !12963–!13012

Reviewed 2026-09-22 using GPT-5.6 Sol.

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Exactly 50 previously unreviewed merge requests were reviewed in this run. Before selection, the available per-run ledgers under `reviewed-mrs-automation/`, the aggregate review tracking, and `reviewed-mrs.md` were consulted to reconstruct the explicit already-reviewed set. The historical !17571–!17620 batch was preserved and counted. The preceding ledger had inspected !13012 only as a frontier probe and explicitly did not count it as reviewed. The selected set happens to be contiguous, but selection was performed from corpus membership minus the explicit reviewed set rather than by assuming numeric-range coverage.

Outcome summary: 47 merged and 3 closed/unmerged (!13009, !12983, !12978). Merged changes were weighted most heavily. The three closed proposals were down-weighted; !12978 is explicitly a duplicate of already-merged !12975, while !12983 was an abandoned optimization and !13009 an unmerged display-filter type-generalization proposal.

## Durable notebook updates

- `unit-contract-conventions.md`: interfaces can represent the same quantity in different units even when they use the same integer type. Guy Harris's merged !13003 plus stable backports !13004, !13005, and !13008 fix FCS-length propagation across bit, octet/byte, and 16-bit-unit interfaces. Keep units explicit and convert once at boundaries.
- `callback-registration-conventions.md`: callback registration can synchronously replay state that was already read. John Thacker's merged !13006 and release backport !13007 move `capinfos` counter initialization before callback registration so replayed callbacks cannot be erased by a later reset.
- `static-analysis-conventions.md`: do not silence a static analyzer by destroying stronger compiler exhaustiveness diagnostics. Merged !13002 preserves an enum switch without `default` while satisfying initialization analysis; adjacent merged analyzer cleanup !12991/!12982 reinforces the diagnostic-preservation tradeoff.
- `displayed-frame-context-conventions.md`: timestamp-relative predecessor/reference state must advance only on records that actually have timestamps. John Thacker's merged !12996 handles captures mixing timestamped and untimestamped records without letting an untimestamped record corrupt later delta calculations.
- `configuration-precedence-conventions.md`: obsolete a persisted preference when its original dependency semantics have disappeared and its remaining behavior is confusing or suppresses explicit invocation input. John Thacker's merged !12973 and release backport !12986 remove the historical `Only use the profile "hosts" file` behavior that could unexpectedly defeat `tshark -H`.
- `uat-callback-conventions.md`: John Thacker's merged !12967 documents that UAT `update_cb` mutation is persistent only if the same transformation is applied to copied/new records, e.g. by the copy callback invoking the update callback. Prefer validation-only update callbacks or make normalization part of the copy/save lifecycle too.

## Strong findings retained without duplicate notebook rules

- !13012 restores a display-filter VM jump removed as apparently redundant; nested function calls showed that the instruction still encoded necessary control-flow/stack semantics. Treat control-flow simplification as an invariant proof problem, not a local instruction-count optimization.
- !13010's review reinforces UI information hierarchy: detailed capture metadata such as Decryption Secrets Block information belongs naturally in Capture File Properties rather than automatically crowding the always-visible status bar.
- !12976 fixes `ADD_CUSTOM_CMAKE_INCLUDE`: test whether a CMake macro received arguments using argument-count/presence semantics (`ARGC`), not by evaluating arbitrary `ARGN` content as a boolean expression.
- !12966 tightens the RF4CE heuristic and then adjusts it to avoid becoming over-restrictive. This reinforces the existing heuristic-dissector balance: require enough independent structure to avoid false claims without rejecting legitimate protocol variants.
- !12969 fixes address-object cleanup around IPv4/IPv6 display conversion and reinforces existing resource-ownership guidance: use the project address helpers and pair owned address construction with `free_address()` where required.
- !12974/!12975 fix TFS NULL handling through the accessor API rather than direct structure dereference; !12978 was closed because the same release fix was already present in !12975.
- !12979 contains useful ordinary dissector review: Anders Broman questioned whether the encapsulated FlexRay frame should be delegated to `packet-flexray.c`, while the author explained the distinct Bus Mirroring representation; Jaap Keuter caught an incorrect bit mask before merge. The accepted result is protocol-specific rather than a new architecture rule.
- !12977's extensive ICMPv6 PvD review favors the appropriate modern tree/bitmask helper instead of duplicating extraction and display work, corroborating existing tree-API reuse guidance.
- !12970's `value_string_ext` conversion reinforces using indexed lookup forms for large stable value tables where ordinary linear lookup is needlessly expensive.
- !12971 sorts generated ISOBUS data in the generator, reinforcing that deterministic/canonical ordering belongs in generation rather than relying on incidental source CSV ordering.
- !12992/!12993 update zstd compatibility, !12994/!12995 adjust CI rules, !12997/!12998 temporarily disable problematic GLib tests, and !12999/!13000 correct libpcap capability annotations. These are accepted but narrow build/compatibility changes and were not promoted into broader notebook rules.
- !12981/!12984 (LAPD address metadata), !12985/!12987 (CIP I/O protocol-tree parent), !12964 (JSON Info-column text), !12965 (clear stale Byte View highlighting), and !12968/!12972 (display-filter documentation) are merged correctness/documentation changes with no additional cross-cutting rule beyond existing guidance.
- !12963 is the release-4.2 documentation/example backport of the C99-type migration. It is accepted evidence for the project's standard-type direction but does not add a new convention beyond already-recorded type modernization guidance.

## Down-weighted closed proposals

- !13009 — closed/unmerged display-filter function argument generalization. Guy Harris's discussion that semantic validation can produce better errors than forcing everything into syntax remains useful review context, but the proposal is not accepted implementation precedent.
- !12983 — closed/unmerged immediate-mode display-filter CALL optimization. The author closed it after determining `arg_count` was not actually unused; it is evidence to verify apparent dead parameters/instructions before optimizing them away, not a model implementation.
- !12978 — closed duplicate release-4.2 TFS NULL fix; Pascal Quantin explicitly noted that !12975 had already done it. The merged !12975 is the authoritative implementation evidence.

## Exact reviewed MR set

`!13012, !13011, !13010, !13009, !13008, !13007, !13006, !13005, !13004, !13003, !13002, !13001, !13000, !12999, !12998, !12997, !12996, !12995, !12994, !12993, !12992, !12991, !12990, !12989, !12988, !12987, !12986, !12985, !12984, !12983, !12982, !12981, !12980, !12979, !12978, !12977, !12976, !12975, !12974, !12973, !12972, !12971, !12970, !12969, !12968, !12967, !12966, !12965, !12964, !12963`

## Frontier

`!12962` (`GTP, GTPv2: use wmem_maps instead of GHashTables`) exists in the same corpus commit and is merged. It was fetched only to verify that the corpus continues below this batch and is **not** counted as reviewed here. Absent newly scraped higher-numbered unreviewed material, !12962 is the next descending candidate.
