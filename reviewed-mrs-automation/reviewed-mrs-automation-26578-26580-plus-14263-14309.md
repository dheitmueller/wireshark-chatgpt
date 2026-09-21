# Wireshark MR review automation ledger: !26580-!26578 plus !14309-!14263

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`
Notebook starting commit: `b47988d6a2a6c991a3de052460387b187455f226`
Reviewed count: **50**
Status mix: **46 merged, 2 open/unmerged (`!26580`, `!26579`), and 2 closed/unmerged (`!14294`, `!14276`)**.

## Selection and duplicate avoidance

Before selecting this batch, the reviewed set was reconstructed from the available tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md`, the aggregate automation tracking, and the per-run files under `reviewed-mrs-automation/`. The historical **!17571-!17620** batch remains explicitly preserved and counted. In particular, the earlier mixed ledger `reviewed-mrs-automation-14710-14748-plus-26567-26577.md` proves that the then-current corpus contained and reviewed !26567-!26577 while !26578 and !26579 were absent.

The corpus advanced from `1c7ad509887ee25079a7865cc62ba18cba06f49a` to `ddcaa22b51c68f594e425a23388c3a2086813054`. The new snapshot contains !26578, !26579, and !26580; `mr_26581.json` is absent, and no later 2659x entry was found. Exact reviewed-set subtraction therefore makes those three newly arrived MRs the highest-numbered unreviewed entries, followed by the prior descending frontier. The fifty selected MRs are intentionally non-contiguous:

`!26580, !26579, !26578, !14309, !14308, !14307, !14306, !14305, !14304, !14303, !14302, !14301, !14300, !14299, !14298, !14297, !14296, !14295, !14294, !14293, !14292, !14291, !14290, !14289, !14288, !14287, !14286, !14285, !14284, !14283, !14282, !14281, !14280, !14279, !14278, !14277, !14276, !14275, !14274, !14273, !14272, !14271, !14270, !14269, !14268, !14267, !14266, !14265, !14264, !14263`.

While validating the former frontier before the newly scraped high-numbered entries were noticed, !14262-!14260 were probed. They are deliberately **not** counted as reviewed, are not part of this ledger, and remain eligible for a future exact selection.

## Review notes

| MR | State | Review | Durable assessment |
| --- | --- | --- | --- |
| !26580 | open, release-4.4 | Scanned / down-weighted | Stable-branch cherry-pick of !26578. Pipeline passed but it remains unmerged with no discussion; no independent convention promoted. |
| !26579 | open, release-4.6 | Scanned / down-weighted | Stable-branch cherry-pick of !26578. Pipeline passed but it remains unmerged with no discussion; no independent convention promoted. |
| !26578 | merged, master | Deep | Gerald Combs changes the TLS CT log-ID generator to use the URL hostname as a more descriptive fallback when a log description is absent. Author/merger authority is high, but this is a narrow generated-data presentation improvement with no new cross-cutting convention. |
| !14309 | merged | Scanned | Falco bridge integer-cast cleanup; useful type correctness maintenance, no distinct new convention. |
| !14308 | merged, stable | Scanned / corroborating | Stable backport of the nullable `wmem_map` helper semantics accepted in !14305. |
| !14307 | merged, stable | Scanned / corroborating | Stable backport of !14305. |
| !14306 | merged, stable | Scanned / corroborating | Stable backport of !14305. |
| !14305 | merged, master | Deep / promoted | Gerald Combs centralizes useful NULL-as-empty semantics in `wmem_map` query/removal/iteration helpers while insertion still requires a real map. API comments explicitly distinguish “May be NULL” from “Must not be NULL”. Added to `container-api-conventions.md`. |
| !14304 | merged | Discussion-focused / historical | Custom-column format parsing change that split from the right. Later already-reviewed fixes refined caller/identity behavior, so this is retained as precursor evidence rather than a new rule. |
| !14303 | merged | Scanned | DICOM generated/read-only table constification; straightforward cleanup. |
| !14302 | merged | Corroborating | Expands the license-check scan horizon after !14295 moved a generated marker beyond an arbitrary old limit. Supports keeping tooling assumptions aligned with generated-source structure. |
| !14301 | merged | Scanned | Sysdig/Falco highlighting/UI maintenance; no cross-cutting lesson. |
| !14300 | merged | Scanned | Qt protocol-tree null safety; narrow defensive fix. |
| !14299 | merged | Scanned / corroborating | Zigbee NWK GP heuristic stops requiring an invalid PAN-broadcast assumption. Reinforces existing heuristic selectivity/protocol-invariant guidance. |
| !14298 | merged | Scanned | Spelling/comment cleanup only. |
| !14297 | merged | Scanned | IPv6 conversation support with stream identifiers; feature work, no additional convention extracted. |
| !14296 | merged | Scanned | LI5G terminology/filter-name cleanup; no new cross-cutting rule. |
| !14295 | merged, master | Deep / promoted | During DICOM standards regeneration, Guy Harris noticed previously known OIDs disappearing. Investigation showed the newer authoritative source had moved a UID family into another table, requiring generator/source adaptation rather than accepting silent data loss. Added to `authoritative-source-preservation-conventions.md`. |
| !14294 | closed, unmerged | Down-weighted | Earlier DICOM constification attempt; superseded by merged !14303 and the accepted !14295 regeneration work. |
| !14293 | merged | Scanned | Conversation debug/type/assertion cleanup; no new rule. |
| !14292 | merged | Scanned | Qt status-bar wording/UI change. |
| !14291 | merged | Scanned | M3UA TCP port/reassembly support; protocol feature with no new general lesson. |
| !14290 | merged | Scanned | Logray UI update; no reusable convention extracted. |
| !14289 | merged | Scanned | DCE/RPC Netlogon improvements; protocol-specific. |
| !14288 | merged, stable | Scanned / corroborating | Stable backport of !14287's display-filter NULL semantics. |
| !14287 | merged, master | Deep / promoted | John Thacker fixes `min()`/`max()` and stack cleanup for absent-field values represented by NULL `GPtrArray`s after an internal representation change. Tests prove missing values remain absent through nested `len`, slice, arithmetic, and aggregate expressions rather than becoming zero/constants. Added to `dfilter-conventions.md`. |
| !14286 | merged | Scanned | LDAP improvements; protocol-specific. |
| !14285 | merged | Scanned | Kerberos improvements; protocol-specific. |
| !14284 | merged | Scanned | Comment/spelling cleanup. |
| !14283 | merged | Discussion-focused | Modbus write-data/bit presentation changes prompted review of how boolean bit position and filtering are exposed; useful field/filter UX evidence, but no stronger rule than existing field semantics guidance. |
| !14282 | merged | Scanned | Logray author-data update. |
| !14281 | merged | Scanned | Falco bridge conversation updates including 64-bit conversation data; no new rule beyond existing exact-width/state guidance. |
| !14280 | merged | Scanned | DHCP option 108 interpretation; protocol-specific. |
| !14279 | merged | Deep / corroborating | Large conversation-deinterlacing series with extensive tester feedback exposed stream-number/filter interactions and later cleanup. Useful evidence for conversation identity/state testing, but stronger later notebook guidance already covers those principles. |
| !14278 | merged | Scanned | Typo/trivial cleanup. |
| !14277 | merged | Scanned | PDCP-NR integrity data support; protocol feature. |
| !14276 | closed, unmerged | Down-weighted | Draft predecessor to the accepted PDCP-NR work; superseded by !14277. |
| !14275 | merged | Discussion-focused | Welcome-page recent-files preference discussion distinguishes hiding one UI presentation from globally disabling recent-file history. Useful UI-semantics evidence but not promoted. |
| !14274 | merged, master | Deep / promoted | John Thacker fixes GeoIP map generation to traverse the `TrafficDataFilterProxy`, map proxy indexes to source indexes, and separately expose proxy/source model helpers. Added to `qt-model-conventions.md`: view-relative output must honor the filtered/sorted proxy. |
| !14273 | merged | Scanned | Field identifier/spelling correction. |
| !14272 | merged | Discussion-focused / corroborating | DNS Query-Response stats review includes normal rebase/force-push workflow discussion. Corroborates existing submission guidance; no duplicate rule added. |
| !14271 | merged | Scanned | Automatic generated/data update. |
| !14270 | merged | Scanned | Automatic generated/data update counterpart. |
| !14269 | merged | Scanned | Automatic generated/data update counterpart. |
| !14268 | merged | Scanned | Automatic generated/data update counterpart. |
| !14267 | merged | Deep / corroborating | WSLua `getnum` adds a pre-operation integer-overflow bound check for a recurrence of CVE-2020-14147. Strong arithmetic-safety evidence already covered by existing notebook rules. |
| !14266 | merged | Deep / corroborating | John Thacker adds an explicit OPC UA unknown message mode and preference-based signature fallback when the opening negotiation was not captured, instead of conflating unknown with unsigned. Strongly corroborates existing explicit-unknown state-model guidance. |
| !14265 | merged | Scanned | Capture-file filter path reuses compiled display-filter code rather than recompiling; focused performance/state improvement. |
| !14264 | merged | Scanned | Falco bridge buffer-format work; no new cross-cutting rule. |
| !14263 | merged | Discussion-focused | Gerald Combs and John Thacker discuss follow-dialog delta-time presentation and QPlainTextEdit layout costs on streams with little/no whitespace. Useful UI/performance context, but no separate durable convention promoted. |

## Notebook changes promoted from this run

1. `container-api-conventions.md` — nullable container operations may encode “absent means empty” once in the shared API when that semantic is universal, while operations such as insertion can still require a concrete container (!14305 with stable backports !14306-!14308).
2. `authoritative-source-preservation-conventions.md` — standards/generated-data upgrades must audit surprising removals and adapt the generator when the authoritative source reorganizes tables instead of silently dropping historical identifiers (!14295, with Guy Harris's review given especially high weight).
3. `dfilter-conventions.md` — preserve absent-field semantics across internal value-container refactors, and test missing values through nested/intervening operations (!14287/!14288).
4. `qt-model-conventions.md` — operations defined by the current filtered/sorted view must traverse the proxy model and map to source indexes for canonical domain data rather than bypassing the view state (!14274).

The high-numbered !26578 master change and still-open !26579/!26580 backports were reviewed first because the new corpus snapshot made them the highest-numbered unreviewed entries. They did not displace the stronger general lessons above.

## Continuity

`mr_14262.json` exists at this corpus commit and was intentionally not counted in this run, so the corpus is **not exhausted**. If no newer previously-unreviewed MRs are added before the next run, the next descending candidate is **!14262**. The scraping tool therefore does not need to be restarted.