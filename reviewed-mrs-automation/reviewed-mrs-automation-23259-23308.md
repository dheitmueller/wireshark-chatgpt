# Automated Wireshark MR review: !23259-!23308

## Corpus identity

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`
- Review direction: newest available previously-unreviewed MRs toward older MRs
- MRs reviewed in this run: **50**

## Selection and de-duplication

Before selecting this batch, all available review tracking in `dheitmueller/wireshark-chatgpt` was consulted, including `reviewed-mrs.md`, the supplemental aggregate automation ledger, and the per-run files under `reviewed-mrs-automation/`. Candidate MR numbers were checked against tracking individually rather than treating ledger filename ranges as proof that every MR inside an interval had been reviewed. The supplemental aggregate ledger was also used to account for individually reviewed MRs in ranges not represented by a single per-run filename.

The historical reviewed batch **!17571-!17620** remains part of the already-reviewed set and was explicitly preserved and counted through `reviewed-mrs-automation/reviewed-mrs-automation-17571-17620.md`.

No existing tracking entry referenced !23308 or any !232xx MR in this candidate region. The fifty highest-numbered corpus MRs not already represented in review tracking were therefore confirmed to be **!23308 through !23259 inclusive**.

## Exact MRs reviewed

1. !23308
2. !23307
3. !23306
4. !23305
5. !23304
6. !23303
7. !23302
8. !23301
9. !23300
10. !23299
11. !23298
12. !23297
13. !23296
14. !23295
15. !23294
16. !23293
17. !23292
18. !23291
19. !23290
20. !23289
21. !23288
22. !23287
23. !23286
24. !23285
25. !23284
26. !23283
27. !23282
28. !23281
29. !23280
30. !23279
31. !23278
32. !23277
33. !23276
34. !23275
35. !23274
36. !23273
37. !23272
38. !23271
39. !23270
40. !23269
41. !23268
42. !23267
43. !23266
44. !23265
45. !23264
46. !23263
47. !23262
48. !23261
49. !23260
50. !23259

## Review notes

| MR | Outcome / review depth | Durable evidence |
| --- | --- | --- |
| !23308 | Scanned, merged | S1AP v19.1.0 generated/spec upgrade; no new cross-cutting convention. |
| !23307 | Scanned, merged | GCC clobber workaround accepted by John Thacker; narrow compiler-portability maintenance. |
| !23306 | Scanned, merged | LPP v19.1.0 generated/spec upgrade. |
| !23305 | Deep, merged | Removes impossible `tvb_reported_length_remaining()` comparisons; reinforces already-recorded remaining-length/type-domain guidance. |
| !23304 | Scanned, merged | E1AP v19.1.0 generated/spec upgrade. |
| !23303 | Scanned, merged | Rebased Wireshark's FindLua on upstream CMake and reapplied local patches for Lua 5.5 detection; build-maintenance/provenance example. |
| !23302 | Scanned, merged | F1AP v19.1.0 generated/spec upgrade. |
| !23301 | Discussion-focused, merged | Large Qt recent-file/WorkspaceState model migration. Roland Knall and Michael Mann discussed competing state/preference-layering directions; useful architecture context, but transitional disagreement makes it unsuitable as a finalized general rule. |
| !23300 | Scanned, merged | MariaDB progress-reporting protocol extension; focused dissector feature. |
| !23299 | Scanned, merged | AMR-WB RTP Player support with sample-based testing; codec feature rather than a new general convention. |
| !23298 | Scanned, merged | NR RRC v19.1.0 generated/spec upgrade. |
| !23297 | Scanned, merged | Clang Analyzer dead-store cleanup in GTPv2. |
| !23296 | Scanned, merged | Adds protobuf preference to suppress noisy top-level key/value pairs in Info column. |
| !23295 | Deep, merged | John Thacker fixes a WCCP loop where malformed input could supply a huge count while a zero/invalid address length prevented progress. Strongly reinforces existing parser-progress and hostile-input resource-bound rules. |
| !23294 | Deep, merged | John Thacker fixes WSLua FuncSaver lifetime: objects stored directly in `lua_State` must be freed before that state/thread is reset, not later with packet cleanup. Promoted to `memory-lifetime-conventions.md`. |
| !23293 | Scanned, merged | SMB reparse-point dissection enhancement. |
| !23292 | Scanned, merged | Arch Linux CI update following plugin split. |
| !23291 | Scanned, merged | Further accepted double-fetch elimination; corroborates existing fetch-once/return-value-helper guidance. |
| !23290 | Scanned, merged | Falco/CMake library-variable cleanup using a Windows generator expression. |
| !23289 | Deep, merged | New MySQL X dissector. Jaap Keuter required `tcp_dissect_pdus()` rather than assuming TCP supplies complete PDUs; strongly corroborates existing TCP reassembly guidance. |
| !23288 | Scanned, merged | LTE RRC v19.1.0 generated/spec upgrade. |
| !23287 | Deep, merged | John Thacker removes a duplicate/wrong stringz length calculation and relies on the encoding-aware TVB API; reinforces shared-helper and semantic-return guidance. |
| !23286 | Deep, merged | Broad conversion of `tvb_reported_length_remaining()` to unsigned return semantics; already covered by type/remaining-length notebook guidance. |
| !23285 | Deep, merged | Tactical unsaved-frame guard for Export PDUs. Guy Harris explicitly identifies the deeper semantic problem: an Export operation should not replace the active capture the way Save As does. Recorded here with high authority, but not promoted as fully accepted architecture because this MR mitigates rather than completes that redesign. |
| !23284 | Scanned, merged | `_expert_remaining` / unsigned-offset conversion; existing guidance. |
| !23283 | Scanned, merged | NSIS Stratoshark plugin-install correction. |
| !23282 | Scanned, merged | Removes an impossible Zigbee reported-length test; existing API semantics. |
| !23281 | Deep, merged | PPI ptvcursor use-after-free correction; corroborates existing ownership/lifetime rules. |
| !23280 | Scanned, merged | Falco libs update, debug-library handling, and Windows Stratoshark CI enablement. |
| !23279 | Scanned, merged | NSIS/WiX plugin-path correction. |
| !23278 | Scanned, merged | pkg-config metadata correction. |
| !23277 | Deep, merged | Explicitly frees UI preference modules at exit so LeakSanitizer/Valgrind remain useful even where process teardown would reclaim memory. Reinforces sanitizer-clean lifecycle hygiene. |
| !23276 | Deep, merged | Restores unsigned Soulseek fields based on current protocol semantics and removes redundant reported-length checks; existing signedness/bounds guidance. |
| !23275 | Scanned, merged | Expert-remaining / unsigned-offset cleanup. |
| !23274 | Scanned, merged | Splits Wireshark and Stratoshark plugin build directories; build/product organization. |
| !23273 | Discussion-focused, merged | Lua plugin directories containing `init.lua` become packages. John Thacker requested testing by experienced Lua users; merged after domain review. Useful domain-validation example, no new generic rule. |
| !23272 | Scanned, merged | Refreshes Soulseek protocol-reference URLs, including archival links. |
| !23271 | Deep, merged | Adds Python `__main__` guards for ProcessPoolExecutor under Python 3.14; corroborates previously recorded spawn/multiprocessing portability guidance. |
| !23270 | Deep, merged | Encodes an internal nonzero-length precondition with an assertion to make Coverity's assumed path explicit; existing assertion/static-analysis guidance. |
| !23269 | Deep, merged | Frees the ptvcursor structure itself when its allocation scope is known; reinforces explicit ownership/cleanup. |
| !23268 | Scanned, merged | Removes `-1` sentinel usage in TLS ahead of unsigned remaining-length migration. |
| !23267 | Deep, merged | Makes/removes ineffective negative-length tests and removes `if (tree)` guards that suppressed bounds exceptions; reinforces the rule that safety checks cannot depend on tree construction. |
| !23266 | Deep, merged | Similar remaining-length cleanup; rewrites logcat heuristic checking and relies on TVB bounds-aware APIs. |
| !23265 | Scanned, merged | Automated master data/translation/number update. |
| !23264 | Scanned, merged | Automated release-4.6 data update. |
| !23263 | Scanned, merged | Automated release-4.4 data update. |
| !23262 | Deep, merged | Documents the historical change from `-1` to `0` for out-of-range reported length and removes tree-dependent bounds suppression; strong corroboration of existing TVB contract guidance. |
| !23261 | Scanned, merged | Corrects CI handling of `TEST_EXTRA_ARGS` as a CMake list. |
| !23260 | Deep, merged | Coverity-driven OOB fix: validate loop index before array access and terminate on `>= max`; straightforward memory-safety invariant. |
| !23259 | Scanned, merged | Removes impossible DHCP checks based on obsolete `tvb_reported_length_remaining()` negative-return semantics. |

## Durable finding promoted to the notebook

### !23294 — Tear down state-owned helpers before resetting their owner

Merged master MR !23294, authored and merged by John Thacker, fixes an illegal access in WSLua. FuncSaver objects are stored directly in the `lua_State`, unlike neighboring allocations that are cleared with packet-oriented `clear_outstanding_*` cleanup. Because the owning Lua thread/state is reset earlier, the FuncSavers must be freed before that reset rather than deferred until the end of `packet_info` lifetime. The accepted implementation also uses container-owned destroy callbacks to simplify teardown.

The bug had existed while tests happened to pass because garbage-collection and allocation details masked the invalid access; a later increase in state memory use exposed it. This makes the lifecycle lesson stronger than a mere leak cleanup: the semantic owner/reset boundary, not incidental allocator behavior, determines when dependent objects become invalid.

Promoted to `memory-lifetime-conventions.md`.

## Strong corroborating and contextual evidence not duplicated as new rules

- **!23295** strongly reinforces the existing parser-progress rule: attacker-controlled loop counts are dangerous when malformed lengths can prevent the parser offset from advancing.
- **!23289** contains direct Jaap Keuter review requiring `tcp_dissect_pdus()` because TCP dissectors cannot assume one call contains a complete PDU. Existing `dissector-conventions.md` already captures this class of TCP reassembly rule.
- **!23287, !23291** reinforce existing fetch-once/shared-helper/API-return guidance.
- **!23286, !23284, !23282, !23276, !23275, !23268, !23267, !23266, !23262, and !23259** are a coherent continuation of the accepted unsigned offset/remaining-length migration and related cleanup. They were not duplicated as separate rules.
- **!23271** independently corroborates the already-recorded requirement that Python multiprocessing tooling work under spawn-style semantics and protect process-pool startup with an appropriate `__main__` boundary.
- **!23270** reinforces existing static-analysis guidance: when an impossible path truly represents a programmer precondition, encode the invariant explicitly rather than reshaping correct code around an analyzer's hypothetical state.
- **!23285** includes especially authoritative design feedback from Guy Harris. He distinguishes Export from Save As: exporting should create a secondary artifact without replacing the currently open capture. The merged MR adds the standard unsaved-frame guard because the legacy Export-PDUs path does replace the active capture, so this run records Guy's feedback as strong future-facing GUI semantic guidance while avoiding a claim that the broader redesign was accepted in this MR.
- **!23301** contains a substantive architectural discussion between Roland Knall and Michael Mann about moving `recent` state into Qt models versus generalizing shared preference infrastructure. Because the MR is explicitly an incremental/test-balloon step and the discussion exposes unresolved layering choices, no broad architecture rule was inferred from it.

## Evidence weighting

Every MR in this selected batch was merged in the corpus snapshot, so outcome weighting did not require preferring merged successors over abandoned alternatives within the batch. Generated ASN.1/spec upgrades, automated data refreshes, one-line analyzer cleanups, and packaging maintenance were still counted but received less interpretive weight than substantive correctness changes and human review.

The strongest reusable evidence came from John Thacker's merged lifetime/progress/API work and from direct review by maintainers with relevant domain expertise. Guy Harris's comment on !23285 was given especially high design authority, but its scope was kept distinct from the narrower merged fix so review opinion was not incorrectly presented as an already-landed architectural change.
