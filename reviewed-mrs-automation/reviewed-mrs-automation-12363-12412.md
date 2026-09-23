# Automated MR review: !12412 through !12363

Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Selection and tracking

Before selecting this run, the reviewed set was reconstructed from the available per-run ledgers under `reviewed-mrs-automation/`, `reviewed-mrs.md`, and the supplemental automation tracking already summarized by the newest authoritative run ledger. The historical !17571-!17620 batch remains preserved and counted. The preceding run's lookup of !12412 was a frontier probe only and did not count it as reviewed. No tracking entry marks any MR in !12412 through !12363 as previously reviewed, so these are the fifty highest-numbered previously unreviewed MRs present in this corpus snapshot.

Exact reviewed MR numbers:

`!12412, !12411, !12410, !12409, !12408, !12407, !12406, !12405, !12404, !12403, !12402, !12401, !12400, !12399, !12398, !12397, !12396, !12395, !12394, !12393, !12392, !12391, !12390, !12389, !12388, !12387, !12386, !12385, !12384, !12383, !12382, !12381, !12380, !12379, !12378, !12377, !12376, !12375, !12374, !12373, !12372, !12371, !12370, !12369, !12368, !12367, !12366, !12365, !12364, !12363`

Batch outcome: **48 merged, 2 closed/unmerged** (!12389 and !12377). Merged master changes were weighted most heavily; release-branch cherry-picks and repetitive CI/generated-spec updates were used mainly as corroboration. Closed drafts were treated as discussion evidence only.

## Review notes

| MR | Depth | Review result |
|---|---|---|
| !12412 | Scanned | Merged master. Updates Wireshark's vendored PLY `yacc.py` from 3.8 to 3.11. Large upstream/vendor synchronization with no substantive human review beyond approval; useful maintenance but no distinct Wireshark convention. |
| !12411 | Scanned | Merged release-4.2 cherry-pick of SAToP optional RTP-header handling. Backport of !12401; no additional review lesson. |
| !12410 | Scanned | Merged release-branch GUI change adding release notes to the Help menu. Straightforward cherry-pick/presentation work. |
| !12409 | Scanned | Merged master WSDG correction documenting actual CMake build-type flags: RelWithDebInfo includes `NDEBUG`, Debug does not promise `-O0`, and an over-specific debug flag is removed. Good documentation-correctness maintenance, but no substantive review discussion. |
| !12408 | Scanned | Merged CI change switching macOS builds to Qt 6.5.3. Toolchain/version maintenance. |
| !12407 | Scanned | Merged release-branch CI change generating Sparkle signatures on macOS. Repetitive packaging/signing workflow work. |
| !12406 | Scanned | Merged release-branch variant of the Sparkle-signature CI work. No additional durable lesson. |
| !12405 | Scanned | Merged master/primary Sparkle-signature CI workflow update. Packaging automation, no substantive review convention extracted. |
| !12404 | Scanned | Merged Debian packaging correction for symbol versions; cherry-pick. Packaging-local maintenance. |
| !12403 | Scanned | Merged GitLab CI regex fix. Tooling-local correction. |
| !12402 | Scanned | Merged master Debian symbol-version correction. Packaging-local maintenance. |
| !12401 | Discussion/diff scan | Merged master, authored by Jaap Keuter and merged by Alexis La Goutte. SAToP gains optional RTP presence selection (fixed or heuristic) and places RTP dissection on the correct side of the pseudo-wire control word according to the demultiplexing layer. No substantive human review beyond approval; protocol-specific behavior rather than a new cross-cutting rule. |
| !12400 | Scanned | Merged Debian packaging change adding missing symbols, a cherry-pick. No new convention. |
| !12399 | Scanned | Merged GitLab CI filename correction. Tooling-only. |
| !12398 | Scanned | Merged display-filter-reference workflow update. CI/documentation automation; part of a repetitive series. |
| !12397 | Scanned | Merged display-filter-reference workflow update. Same series, no distinct lesson. |
| !12396 | Scanned | Merged macOS signature workflow update. Packaging/CI maintenance. |
| !12395 | Scanned | Merged display-filter-reference workflow update. Same series, no distinct lesson. |
| !12394 | Deep / promoted | Merged master, authored and merged by João Valverde. Removes a redundant runtime magic value from the statically typed `stnode_t` wrapper while retaining runtime magic on dynamically typed payloads. Simplification exposed a real assertion bug: `OPER_MAGIC` had been checked against the wrapper rather than the `oper_t` payload. Promoted to `runtime-type-invariant-conventions.md`: rely on compile-time type safety where it fully describes the wrapper; reserve runtime tags for erased/dynamic payloads and assert against the representation that owns the tag. |
| !12393 | Scanned | Merged generated LPP dissector upgrade to 3GPP v17.6.0. Specification refresh; no substantive review discussion. |
| !12392 | Scanned | Merged generated F1AP dissector upgrade to v17.6.0. Specification refresh. |
| !12391 | Scanned | Merged generated E1AP dissector upgrade to v17.6.0. Specification refresh. |
| !12390 | Scanned | Merged generated XnAP dissector upgrade to v17.6.0. Specification refresh. |
| !12389 | Discussion-focused, closed/unmerged | John Thacker's FlowLayout proof-of-concept for the Qt search frame. Roland Knall objected both to copying Qt's example implementation verbatim (license/provenance and project-ownership concerns) and to importing web/mobile-style responsive behavior that could violate desktop platform UX expectations. John noted BSD-3-Clause compatibility but also acknowledged that he lacked a Mac to validate the platform-specific UI issue. Useful negative/provisional evidence for provenance and target-platform UX testing, but deliberately down-weighted because the draft was never merged. |
| !12388 | Scanned | Merged generated NGAP dissector upgrade to v17.6.0. Specification refresh. |
| !12387 | Scanned | Merged generated NR RRC dissector upgrade to v17.6.0. Specification refresh. |
| !12386 | Scanned | Merged generated X2AP dissector upgrade to v17.6.0. Specification refresh. |
| !12385 | Scanned | Merged generated LTE RRC dissector upgrade to v17.6.0. Specification refresh. |
| !12384 | Scanned | Merged generated NAS 5GS dissector upgrade to v17.12.0. Specification refresh. |
| !12383 | Scanned | Merged generated NAS EPS dissector upgrade to v17.11.0. Specification refresh. |
| !12382 | Scanned | Merged GitLab CI path correction. Tooling-only. |
| !12381 | Scanned | Merged master Qt search-frame spacing adjustment for macOS appearance. UI-local presentation change; no substantive review discussion. |
| !12380 | Deep / corroboration | Merged master, authored by John Thacker. Extends Find Packet so repeated matches can be traversed within Packet Bytes and grows reverse-search primitives (`memrchr`, reverse mempbrk, reverse string/binary search). Strongly corroborates the existing `search-offset-conventions.md` direction that search state must represent positions/ranges in a way that supports repeated and reverse traversal without endpoint ambiguity. No separate rule was needed. |
| !12379 | Scanned | Merged macOS signature CI update. Packaging automation. |
| !12378 | Corroboration | Merged master, authored and merged by John Thacker. Reuses `ws_memmem()` for binary Find rather than maintaining a separate byte-subsequence loop. Reinforces shared-helper reuse and the search cleanup series. |
| !12377 | Discussion-focused, closed/unmerged | Draft radiotap S1G NDP PS-Poll UDI mask correction. Martin Mathieson explicitly withheld confidence because he could not find the authoritative field definition and asked protocol/domain experts for verification; Albert Chuang supplied the expected mask and Alexis linked the radiotap mailing-list discussion. Alexis also requested separating indentation cleanup from the functional mask fix for backportability. Good corroboration for authoritative-source verification and focused backportable commits, but down-weighted because the MR was closed unmerged. Guy Harris was requested for review here, but the corpus discussion does not contain substantive Guy feedback to weight. |
| !12376 | Deep / corroboration | Merged master, authored and merged by John Thacker. Optimizes portable `ws_memmem()` and deliberately makes the zero-length-needle result match GNU `memmem()`/standard `strstr()` behavior. Strong independent corroboration of the notebook's existing compatibility-helper/empty-value rule: portable or ownership-adjusted replacements must match edge semantics, not merely the common algorithm. |
| !12375 | Scanned | Merged automatic 2023-10-01 data/translation update; description notes one services update failed. Generated/automated maintenance. |
| !12374 | Scanned | Merged automatic 2023-10-01 data/translation update. Repetitive generated maintenance. |
| !12373 | Scanned | Merged automatic 2023-10-01 data/translation update. Repetitive generated maintenance. |
| !12372 | Scanned | Merged automatic 2023-10-01 data/translation update. Repetitive generated maintenance. |
| !12371 | Corroboration | Merged master Find cleanup removes a redundant narrow-character matching function because exact case-sensitive matching is byte matching after encoding. Supports simplifying duplicate search paths once their semantic domains are proven equivalent. No separate convention justified. |
| !12370 | Scanned / corroboration | Merged master, authored and merged by John Thacker. Disables the character-encoding selector unless Find is searching raw Packet Bytes, because packet-list/tree strings have already been converted to UTF-8. Good UI-state correctness: expose a control only where it changes semantics, but too localized for a separate notebook rule. |
| !12369 | Corroboration | Merged release-3.6 backport of the `ws_mempbrk_compile()` initialization fix. The compiler routine clears its complete 256-byte pattern before populating it so stack-local destinations need not arrive pre-zeroed and recompiling does not retain stale bits. The original master MR is !12362, immediately below this run's frontier, so the durable rule is intentionally left for the master change's review rather than promoted from a backport. |
| !12368 | Corroboration | Merged release-4.0 backport of the same `ws_mempbrk_compile()` initialization fix. Same evidence; original master !12362 remains unreviewed for the next run. |
| !12367 | Scanned | Merged release-branch IEEE 1609.2 ASN.1 description/CAMP update. Generated/specification maintenance. |
| !12366 | Scanned | Merged release-branch ITS Collective Perception Service update to ETSI TR 103 562 V2.1.1. Generated/specification maintenance. |
| !12365 | Scanned / corroboration | Merged release-4.2 cherry-pick adding a DHCP seconds-elapsed endianness preference while keeping `Autodetect` as the default/legacy behavior and allowing explicit Big/Little Endian selection. Good compatibility-preserving preference design, but this is a backport and no new general rule was needed. |
| !12364 | Scanned | Merged release-branch spelling cleanup in root/UI directories. Cosmetic maintenance. |
| !12363 | Corroboration | Merged release-4.2 cherry-pick of the `ws_mempbrk_compile()` initialization fix. Same lesson as !12368/!12369; defer promotion until original master !12362 is reviewed. |

## Durable notebook updates

- Added `runtime-type-invariant-conventions.md` from merged master !12394. The rule distinguishes statically known wrapper types from dynamically typed/erased payloads: do not maintain redundant wrapper magic when the compiler already guarantees the type; when runtime magic remains necessary, assert it on the payload representation that actually owns the magic. Commit: `680c4d8f06b3e89da9825b3b582d34b32c724375`.
- !12376 corroborates the existing empty-value/compatibility-helper guidance by matching GNU `memmem()` for an empty needle.
- !12380 corroborates existing search-offset/range guidance through repeated and reverse Packet Bytes searches.
- Closed !12377 corroborates authoritative-source verification and backport-focused scope, but was not used as primary accepted implementation evidence. Closed !12389 was likewise retained only as lower-weight provenance/platform-UX discussion evidence.
- The `ws_mempbrk_compile()` initialization lesson in !12363/!12368/!12369 is noted but intentionally not promoted from the release backports because the merged master original, !12362, is the next unreviewed frontier MR.

## Frontier check

!12362 (`wsutil: memset mempbrk pattern when compiling`) exists in this same corpus snapshot, targets master, and is merged. It was inspected only to establish the next frontier and identify the provenance of the three selected backports; it is **not** counted as reviewed in this run. The corpus therefore has not run out of unreviewed MRs.
