# Wireshark MR Review Automation Ledger — !24459–!24508

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed set from `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, and all available per-run files under `reviewed-mrs-automation/`, using individual MR entries rather than assuming whole numeric ranges were reviewed. The historical !17571–!17620 batch remains explicitly preserved and counted. Existing tracking covers the high-numbered corpus through !24509 (including the separately tracked !25759–!25827 segment), so the fifty highest-numbered corpus MRs absent from the reconstructed set are exactly !24508 through !24459 inclusive.

| MR | Review depth | Outcome / durable evidence |
|---|---|---|
| !24508 | Deep | Merged sharkd stable-branch TVB exception containment; malformed data-source extraction must still leave valid JSON. Corroborates existing error-boundary guidance. |
| !24507 | Scanned | Merged HTTP/2 stable backport correcting a copy/paste NULL check after `g_strstrip()`. Local correctness fix. |
| !24506 | Scanned | Merged HTTP/2 stable backport of the same header-value NULL-check repair. |
| !24505 | Deep | Merged RTITCP registration on the TLS heuristic table in addition to TCP so decrypted TLS payload can reach the heuristic. Useful dispatch evidence; no separate rule promoted. |
| !24504 | Scanned | Merged removal/update of stale dead TVB API code and comments. Hygiene only. |
| !24503 | Scanned | Merged `checkAPIs` policy refresh: obsolete TVB API prohibited, `toupper` prohibited, supported printf length syntax updated. Mechanical checker maintenance. |
| !24502 | Deep | Merged Protobuf URI matcher DoS fix: UAT complexity validation plus explicit recursion limit. Promoted to `protocol-validation-conventions.md`. |
| !24501 | Scanned | Merged macOS package CI dependency update. Build maintenance only. |
| !24500 | Scanned | Merged RTPS checksum-property decoding with capture verification. Protocol-specific enhancement. |
| !24499 | Deep | Merged Heimdal deprecation/build change. John Thacker raised capability-detection vs vendor-detection concerns; retained as portability discussion evidence, not a new accepted rule. |
| !24498 | Scanned | Merged RTPS DDSServiceRequest topic-info enhancement with supplied capture verification. |
| !24497 | Scanned | Merged macOS package CI cleanup after temporary packaging workaround. |
| !24496 | Deep | Merged RTPS zero-length signature handling; Anders Broman recommended using typed proto-tree add-and-return helpers. Corroborates existing decode-once/helper guidance. |
| !24495 | Scanned | Merged RTPS GUID bookkeeping fix sets `GUID_HAS_ENTITY_ID` consistently after assigning the entity. Local state invariant. |
| !24494 | Scanned | Merged RTPS NACK_FRAG topic/GUID propagation. Protocol-specific enhancement. |
| !24493 | Deep | Merged EAP/nested-TLS architecture and conversation-context repair with multiple captures. Strong corroboration of existing nested-context/protocol-depth guidance. |
| !24492 | Scanned | Merged RTPS display-filter abbreviation correction with explicit acknowledgement of compatibility impact. API-compatibility evidence only. |
| !24491 | Scanned | Merged Lua Debugger Variables tree expansion-state preservation. UI-specific behavior. |
| !24490 | Scanned | Merged NAS 5GS specification update. Routine protocol maintenance. |
| !24489 | Scanned | Merged Lua Debugger availability in Stratoshark. Build/UI integration. |
| !24488 | Scanned | Merged HTTP/2 master copy/paste NULL-check fix later backported by !24506/!24507. |
| !24487 | Deep | Merged sharkd master TVB exception containment later backported by !24508. Corroborates existing error-boundary guidance. |
| !24486 | Deep | Merged Lua Debugger source/build reorganization; review ensures shared Qt functionality is also integrated for Stratoshark. Existing source-ownership guidance suffices. |
| !24485 | Deep | Merged release-4.4 backport preventing X.25 conversation selectors from leaking above IP. Corroborates promoted !24476 context-scope rule. |
| !24484 | Deep | Merged release-4.6 backport of the same X.25/IP conversation-scope repair. |
| !24483 | Scanned | Merged DNS/TDS unused-variable cleanup backport. Compiler hygiene only. |
| !24482 | Deep | Merged John Thacker counted-string numeric parser API with tests; avoids manufacturing NUL-terminated copies. Strong corroboration of `bounded-text-numeric-parsing-conventions.md`. |
| !24481 | Scanned | Merged DNS/TDS unused-variable cleanup. Compiler hygiene only. |
| !24480 | Deep | Merged documentation correction: `wmem_strbuf_new_len` copies exactly the requested bytes, including embedded NULs; caller owns source bounds. Bounded-copy semantics evidence. |
| !24479 | Deep | Merged RTPS fix bounds `wmem_strbuf_new_len` input by actual string length rather than assuming a fixed amount is readable. Corroborates bounded-copy guidance. |
| !24478 | Scanned | Merged release-4.4 sharkd filter-cache invalidation on new-file load. Lifecycle corroboration. |
| !24477 | Scanned | Merged release-4.6 backport of sharkd filter-cache invalidation. |
| !24476 | Deep | Merged John Thacker X.25/IP conversation lookup repair. Promoted as layer-scoped conversation-selection metadata in `dissector-context-flow-conventions.md`; implementation is explicitly provisional, principle is durable. |
| !24475 | Scanned | Merged WSLua Debugger Watch section; author openly records substantial AI assistance. Feature-specific, no review-derived convention. |
| !24474 | Deep | Merged broad WSLua `__tostring` standardization: semantic, concise, debugger-safe object representations without pointer/full-content leakage. Promoted to `wslua-diagnostic-representation-conventions.md`. |
| !24473 | Deep | Merged John Thacker sharkd filter-cache reset after successful new-file open. Reinforces session replacement commit-point/lifetime rule. |
| !24472 | Scanned | Merged automatic registry/identifier update on master. Generated data maintenance. |
| !24471 | Scanned | Merged automatic registry/identifier update on release-4.6. Generated data maintenance. |
| !24470 | Scanned | Merged automatic registry/identifier update on release-4.4. Generated data maintenance. |
| !24469 | Deep | Merged ADB reassembly allocation hardening after extended review: grow storage from bytes actually captured/copied instead of packet-declared `data_length`; crafted `0xffffffff` capture tested. Strong corroboration of existing allocation/reassembly safety guidance. |
| !24468 | Scanned | Merged release-4.4 sharkd API fix makes `setcomment.comment` mandatory where the operation has no meaningful omitted-value behavior. |
| !24467 | Scanned | Merged release-4.6 backport of mandatory `setcomment.comment`. |
| !24466 | Deep | Merged RF4CE stable backport favoring TVB remaining-length helpers over subtraction and removing unnecessary snprintf buffering. Arithmetic/bounds corroboration. |
| !24465 | Deep | Merged release-4.4 sharkd `cf_close`/reload lifecycle backport. Corroborates promoted !24462 ordering rule. |
| !24464 | Deep | Merged release-4.6 sharkd `cf_close`/reload lifecycle backport. |
| !24463 | Scanned | Merged master sharkd `setcomment.comment` mandatory-parameter correction, later backported. |
| !24462 | Deep | Merged John Thacker sharkd `cf_close` implementation; open replacement first, then close/reset old capture only on success. Promoted to `state-modeling-conventions.md`. |
| !24461 | Deep | Merged John Thacker generated-documentation correction for exact-length `wmem_strbuf_append_len` semantics and caller bounds responsibility. |
| !24460 | Deep | Merged RTPS fix respects exact-length append semantics instead of treating `append_len` like an up-to string copy. Corroborates bounded-copy guidance. |
| !24459 | Scanned | Merged CMake docs dependency target repair for parallel Xcode builds. Build-system correctness; no new cross-cutting rule. |

## Promoted durable conventions

- !24502: validate complexity of user-configured recursive/backtracking matchers at configuration acceptance time and retain a hard execution-time recursion/work bound.
- !24476 with stable backports !24484/!24485: conversation-selection metadata is layer-scoped; lower-layer circuit/exact selectors must not leak into an upper layer whose conversation identity is different.
- !24462 with backports !24464/!24465, reinforced by !24473/!24477/!24478: acquire a replacement capture successfully before tearing down the current usable session and invalidate derived caches at the same successful lifecycle transition.
- !24474: WSLua debugger-facing object representations should be concise semantic identities, avoid raw pointers/unbounded content, and remain safely inspectable in expired/closed states.

Strong merged work in !24482 (bounded numeric parsing), !24469 (untrusted-length allocation/reassembly), !24466 (remaining-length arithmetic), !24493 (nested TLS context), and !24487/!24508 (TVB exception containment) was intentionally treated as corroboration of existing notebook rules rather than duplicated.