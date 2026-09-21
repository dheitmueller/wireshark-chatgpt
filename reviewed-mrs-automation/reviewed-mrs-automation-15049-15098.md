# Automated MR review: !15098-!15049

Corpus commit: `1c7ad509887ee25079a7865cc62ba18cba06f49a`

Reviewed count: **50**.

Selection was produced by reconciling the existing review tracking on notebook `main`, including `reviewed-mrs.md`, the aggregate automation tracking, and the per-run ledger inventory under `reviewed-mrs-automation/`. The immediately preceding exact ledger ended with !15099 (plus the newly scraped !26566), and the notebook/corpus heads had not advanced since that run. The historical !17571-!17620 batch remains explicitly counted. No numeric-range inference was used to fill holes: the selected corpus entries were checked against the tracked reviewed set individually. The resulting fifty highest-numbered unreviewed MRs are the contiguous set !15098 through !15049.

Exact reviewed MR numbers:

`!15098, !15097, !15096, !15095, !15094, !15093, !15092, !15091, !15090, !15089, !15088, !15087, !15086, !15085, !15084, !15083, !15082, !15081, !15080, !15079, !15078, !15077, !15076, !15075, !15074, !15073, !15072, !15071, !15070, !15069, !15068, !15067, !15066, !15065, !15064, !15063, !15062, !15061, !15060, !15059, !15058, !15057, !15056, !15055, !15054, !15053, !15052, !15051, !15050, !15049`

The accepted material in this batch is dominated by the project-wide GLib-to-standard-C type conversion, a cluster of WSLua documentation fixes, several I/O Graph follow-ups, and a few stable-branch cherry-picks. Stable cherry-picks were weighted mainly as corroboration. No abandoned proposal contributed promoted guidance.

## Durable finding promoted

- **!15097 — tolerant persisted readers, stable canonical writers.** John Thacker's merged UAT change accepts `TRUE`/`FALSE` case-insensitively while intentionally continuing to emit uppercase spellings so configuration remains readable by pre-4.4 Wireshark. The Qt UAT path also retains the older `Enabled` spelling where relevant. This adds a durable rule to `persisted-configuration-compatibility-conventions.md`: broadening harmless reader tolerance does not imply changing the writer's canonical representation; test both old-input/new-reader and new-output/old-reader compatibility.

## Strong corroborating findings

- **!15098 / !15096 / !15095 / !15090 / !15089 / !15087 / !15082 / !15080 / !15078 / !15074 / !15072 / !15060 / !15059 / !15058 / !15057 / !15055 / !15054** are part of the standard-C-type migration. In particular, !15096 fixes an ASan-visible storage error caused by reading `bool` backing storage through a `gboolean *`; it strongly corroborates the existing rule that pointer/backing-storage types must exactly match the owning API/data representation rather than being papered over with casts. !15098 removes the last analogous `gboolean *` cast found by the conversion audit. !15060 received direct Guy Harris approval and reinforces that callback/signal contracts must be converted coherently, not only local variables.
- **!15094** moves libm from final executables to the `ui`/`wsutil` library targets that actually use it and keeps the dependency private. This strongly corroborates `build-conventions.md`: build dependencies belong to the component that semantically owns the use, with CMake visibility matching whether the dependency leaks into the public interface.
- **!15091**, approved and merged by Guy Harris, makes unexported Qt capture-file utility globals `static`. It is especially authoritative corroboration of the existing internal-linkage rule: file-local objects should not have external linkage accidentally.
- **!15086 / !15061 / !15053** are a coherent I/O Graph invalidation/performance cluster. A Y-axis scaling-factor change and a QCPGraph/QCPBars representation switch must invalidate/recalculate graph data, while an unchanged plottable style only requires a replot; rescaling work is restricted to the visible graph where only one graph is visible. These reinforce the notebook's existing rule to separate semantic derived-data invalidation from cheap presentation changes and visibility-driven work.
- **!15068** initializes NTLM session-field lookup slots to `-1` before optional registrar lookup/population, preventing stale/uninitialized values when a lookup does not assign the slot. This independently corroborates the existing optional-output/sentinel initialization guidance in `parser-edge-cases.md`.
- **!15052** tightens MongoDB heuristic recognition around the protocol's signed 32-bit length domain, rejecting bogus negative/out-of-range values before they can underflow or make the heuristic claim unrelated traffic. This reinforces the existing semantic-domain and heuristic-selectivity rules rather than creating a separate convention.
- **!15049 / !15050** are stable-branch copies of the MongoDB offset-advance fix: callers must use the sanitized/effective length returned by the BSON parser instead of advancing with the original untrusted length. They are retained as corroboration; the master-side change lies below this batch and should receive primary weight when reviewed.
- **!15071 / !15070 / !15069** carry the editcap large-packet `memmove` fix across maintained branches, replacing a too-narrow byte-count variable with `size_t`. They reinforce existing width/domain guidance for byte counts and size computations.
- **!15093** is a stable cleanup fix for `ManageInterfacesDialog`: accepted work is performed at the destruction/lifecycle point that is safe with `WA_DeleteOnClose`, rather than racing from the accepted-slot callback. It corroborates existing Qt object-lifetime guidance.
- **!15079**, authored and merged by Guy Harris, places the WSLua `treeitem:add` example at the correct AsciiDoc heading level. The surrounding !15088/!15085/!15084/!15083/!15077/!15076/!15075/!15073 changes carry the same documentation-structure correction across related branches/variants. High-authority but narrow documentation evidence; no separate project-wide rule was necessary.

## Remaining reviewed material

- **!15092** converts the capture-file dialog list path representation away from unnecessary `QString` storage toward byte-oriented data appropriate to the underlying path/API use; type/encoding cleanup, no new architecture rule.
- **!15081** updates Lua documentation references away from obsolete mailing-list material; documentation maintenance.
- **!15067 / !15066 / !15065 / !15064** are automated dependency/manufacturer/documentation data refreshes; reviewed for process impact, with no durable coding rule beyond existing generated/automated-source guidance.
- **!15063** refreshes DocBook/XSL-related documentation/build material; maintenance-only for notebook purposes.
- **!15062** corrects I/O Graph border presentation; UI-specific fix without a broader rule.
- **!15056** suppresses a misleading Protobuf re-inclusion warning for known Google base-type definitions rather than changing protocol semantics; accepted parser-specific compatibility fix.
- **!15051** corrects IP length handling at several call sites. It reinforces the existing requirement to distinguish captured/available, reported, and protocol-declared lengths according to the callee contract.

## Weighting notes

Merged master changes were given the most weight. Stable-branch cherry-picks primarily corroborate their master fixes. Guy Harris's direct involvement in !15091, !15079, and !15060 was given especially high authority; those MRs mostly reinforce conventions already present in the notebook rather than introducing new ones. John Thacker's merged !15097 was promoted because its backward-compatibility rationale is explicit and not already captured by the existing positional-schema rule.

## Notebook update

This run extends `persisted-configuration-compatibility-conventions.md` with the !15097 reader/writer compatibility rule. Other strong findings were checked against existing notebook guidance and recorded here as corroboration rather than duplicating rules already present.

The corpus continues below this batch; `mr_15048.json` should be checked as the next backward-continuation candidate.