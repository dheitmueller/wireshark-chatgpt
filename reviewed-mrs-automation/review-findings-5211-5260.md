# Wireshark MR review findings 5211-5260

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged master changes are weighted most heavily. Stable-branch changes corroborate master behavior. Closed !5224 is lower-weight historical/supersession evidence.

| MR | Outcome | Depth | Review finding |
|---|---|---|---|
| !5260 | merged, master | Deep | João Valverde splits `format_size()`'s unit choice from prefix modifiers: one enum for mutually exclusive units, separate flags for orthogonal prefix policy. Removes the C++ enum-`operator|` workaround and updates tests/callers. Strong API-modeling evidence. |
| !5259 | merged, master | Scanned | Martin Mathieson makes file-local Signal-PDU registration helpers `static`; straightforward linkage/encapsulation cleanup. |
| !5258 | merged, master | Scanned | João Valverde removes unused epan string utilities/exports and moves the still-useful null-string convenience to wsutil. Useful API-surface cleanup, no substantive review discussion. |
| !5257 | merged, master | Discussion-focused | Moves generic string escaping to wsutil, gives it a `ws_` API, wmem ownership, optional quoting, additional escape forms, and tests. Useful shared-utility consolidation, but no substantive maintainer discussion. |
| !5256 | merged, master | Scanned | Spelling cleanup plus checker dictionary updates; no durable rule beyond existing style/checker guidance. |
| !5255 | merged, master | Scanned | Adds Keysight/Ixia NetFlow fields; protocol-specific, Alexis La Goutte said LGTM. |
| !5254 | merged, master | Discussion-focused | Frees the temporary list returned by `g_hash_table_get_keys()` after iteration. Good GLib ownership reminder; elements remain owned by the table. |
| !5253 | merged, release-3.6 | Scanned | Automatic generated/data/release-note update; no durable review lesson. |
| !5252 | merged, release-3.4 | Scanned | Automatic generated/data update; no durable review lesson. |
| !5251 | merged, master | Discussion-focused | Replaces transient heap/epan-scope allocation of integer hash lookup keys with stack locals because lookup does not retain the probe key. Corroborates avoiding needless allocation when API lifetime permits stack storage. |
| !5250 | merged, master | Scanned | Automatic generated/data/translation update; no durable review lesson. |
| !5249 | merged, release-3.4 | Corroborating | Backport of Foundation Fieldbus multi-PDU UDP handling from !5235. Confirms the accepted per-PDU carving/consumption model. |
| !5248 | merged, release-3.6 | Corroborating | Backport of Foundation Fieldbus multi-PDU UDP handling from !5235. |
| !5247 | merged, master | Scanned | Adds the directly required Qt `QRegularExpression` include; ordinary include hygiene. |
| !5246 | merged, release-3.4 | Scanned | Typo correction; no durable lesson. |
| !5245 | merged, master | Deep | Qt 6.2 compatibility series. Jörg Mayer explicitly reports independent macOS compile/run testing under both Qt 5.12.10 and Qt 6.2.1 while also stating he is not attesting to the C++ patch semantics. Strong evidence for clearly scoping what a review/test actually establishes. |
| !5244 | merged, release-3.6 | Scanned | Stable typo correction; no durable lesson. |
| !5243 | merged, master | Deep / corroborating | Fixes HTTP/2 code that failed when built without optional nghttp2 after fake-header work introduced unguarded declarations/callbacks. Strong early evidence for feature-disabled build coverage; later No-Options CI rules are stronger. |
| !5242 | merged, master | Discussion-focused | Moves generic case-insensitive substring and memory-search helpers from epan to wsutil, using native platform implementations when detected. Good layering/portability cleanup. |
| !5241 | merged, master | Discussion-focused | Renames generic-looking `int_compare`/`uint_compare` APIs to `wmem_compare_int`/`wmem_compare_uint`, tightening namespace/ownership clarity. |
| !5240 | merged, master | Deep | Improves display-filter character-constant representation so syntax-tree display preserves meaningful character/escape syntax instead of only raw decimal values; also fixes semantic handling. |
| !5239 | merged, master | Deep | Adds correct semantic checking for character constants on the left side of comparisons and a regression test (`'H' == frame[54]`). Reinforces testing both operand orientations in symmetric-looking language features. |
| !5238 | merged, master | Discussion-focused | Parses Signal-PDU textual data-type configuration once into an enum and switches on that typed value later, avoiding repeated packet-path string comparisons. |
| !5237 | merged, master | Scanned | Removes dead code; no new convention. |
| !5236 | merged, master | Discussion-focused | Signal-PDU counterpart to !5251: hash lookup probe keys are stack values rather than allocate/free temporaries. |
| !5235 | merged, master | Deep | Jaap Keuter adds iteration over multiple Foundation Fieldbus PDUs in one UDP payload. It validates minimum/declared lengths, creates a subset tvbuff per PDU, and makes the inner dissector return its actual consumed offset so the wrapper can advance accurately. Stable !5248/!5249 corroborate. |
| !5234 | merged, master | Discussion-focused | Adds count and binary-search APIs for generated epan introspection enums. Typed/generated lookup improvement; no substantive human review. |
| !5233 | merged, master | Scanned | Adds IP protocol constants to introspection enums; support change for the introspection facility. |
| !5232 | merged, release-3.6 | Corroborating / superseded in part | Backport of !5231 timezone arithmetic restructuring. The arithmetic-domain improvement remains useful, but later !5668 is authoritative for offset sign semantics. |
| !5231 | merged, master | Deep / superseded in part | John Thacker converts broken-down time to epoch seconds before applying timezone arithmetic, avoiding manual carry/borrow across minutes/hours/days. Later !5668 corrects the sign direction, so retain the domain-separation lesson but not the old sign behavior. |
| !5230 | merged, release-3.4 | Corroborating | Backport of the stronger RTMPT termination guard from !5225. |
| !5229 | merged, master | Scanned | Clarifies Windows NT epoch commentary; documentation-only. |
| !5228 | merged, master | Deep | Deletes a separate tvbuff ISO-8601 parser and routes through shared `iso8601_to_nstime()`, reducing duplicated parsing semantics and platform differences. |
| !5227 | merged, master | Historical / superseded | Moves Wiretap init/cleanup underneath epan. This was later explicitly reverted by merged !5683 after an exit crash. Use !5227 as negative lifecycle-history evidence only; !5683 is authoritative. |
| !5226 | merged, release-3.6 | Corroborating | Stable backport of !5225 RTMPT termination guard. |
| !5225 | merged, master | Deep | John Thacker adds a conservative break when RTMPT sequence wrap makes the tree traversal's monotonic assumptions unsafe, explicitly prioritizing guaranteed termination even if rare valid wrap cases dissect less completely. |
| !5224 | closed, unmerged | Down-weighted | Proposed lemon/sysroot workaround. Alexis La Goutte asked not to submit from the contributor's master branch; Anders Broman later marked it replaced by !15784. Workflow/supersession evidence only. |
| !5223 | merged, master | Scanned | Adjusts MSVC `-Zc:__cplusplus` handling so C++17 selection works. Toolchain-specific fix, later baseline guidance is stronger. |
| !5222 | merged, release-3.6 | Corroborating | Stable backport of !5219 ISO-8601 bounds hardening. |
| !5221 | merged, master | Scanned | Typo correction; no durable lesson. |
| !5220 | merged, master | Scanned | Fixes a stale filename in a header comment; later !5217 provides the stronger policy of avoiding filename duplication in Doxygen `@file` markers. |
| !5219 | merged, master | Deep | John Thacker prevents out-of-bounds fixed-position probing in ISO-8601 parsing by validating the year prefix first, selecting Basic vs Extended syntax safely, and carrying that mode forward. Also notes that `sscanf` numeric conversions accept signs and can be too permissive for strict grammar. |
| !5218 | merged, release-3.6 | Deep / corroborating | Pascal Quantin removes an unnecessary preference-change handoff callback that re-registers WebSocket in the TCP table and produces a duplicate-registration warning. Preference apply callbacks should exist only for state that actually needs rebinding. |
| !5217 | merged, master | Deep | Jaap Keuter and Gerald Combs converge on bare Doxygen `@file`: do not duplicate filenames that tooling can derive and that go stale after renames. Jaap also argues that sweeping documentation changes should follow an agreed, maintainable policy. |
| !5216 | merged, release-3.4 | Scanned | Adds branch name to fuzz failure reports; useful provenance but no new cross-cutting rule. |
| !5215 | merged, release-3.6 | Scanned | Stable counterpart of fuzz-report branch provenance. |
| !5214 | merged, release-3.4 | Corroborating | RTMPT sequence-number comparison uses wrap-aware TCP sequence semantics rather than raw integer ordering. |
| !5213 | merged, release-3.6 | Corroborating | Same RTMPT wrap-aware sequence comparison on release-3.6. |
| !5212 | merged, master | Discussion-focused | Master fuzz-report provenance change: include branch name when available so retained failures identify the tested branch. |
| !5211 | merged, master | Scanned | Removes outdated/confusing Visual C++ redistributable remediation text from NSIS failure reporting; focused installer UX cleanup. |

## Strongest durable themes

1. Model independent API dimensions independently (!5260).
2. For datagrams that can contain multiple PDUs, carve validated per-PDU tvbuffs and propagate actual consumption (!5235, !5248, !5249).
3. A robustness guard may intentionally choose guaranteed termination over perfect recovery when malformed/ambiguous state defeats a traversal invariant (!5225, !5226, !5230).
4. Validate textual prefixes before fixed-position separator probes, and normalize calendar values before timezone arithmetic; later !5668 remains authoritative for sign direction (!5219, !5222, !5231, !5232).
5. Library lifecycle ownership can be invalidated by shutdown ordering even after an earlier merged centralization; !5227 is superseded by !5683.
6. Preference apply/handoff callbacks must not repeat registration side effects unnecessarily (!5218).
7. Let documentation tooling derive filenames rather than maintaining redundant header metadata, and establish maintainable policy before broad mechanical documentation sweeps (!5217).
8. Optional-feature-off builds are necessary to catch declarations and callbacks accidentally escaping dependency guards (!5243).
