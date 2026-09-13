# Wireshark MR automation review: !21141–!21190

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`
Review direction: newest to oldest

## Exact MRs reviewed

`!21190 !21189 !21188 !21187 !21186 !21185 !21184 !21183 !21182 !21181 !21180 !21179 !21178 !21177 !21176 !21175 !21174 !21173 !21172 !21171 !21170 !21169 !21168 !21167 !21166 !21165 !21164 !21163 !21162 !21161 !21160 !21159 !21158 !21157 !21156 !21155 !21154 !21153 !21152 !21151 !21150 !21149 !21148 !21147 !21146 !21145 !21144 !21143 !21142 !21141`

Count: **50**.

The historical !17571–!17620 batch remains part of the reviewed set and was preserved when selecting this batch. Selection was made from the union of the per-run ledgers and the sparse `reviewed-mrs.md`, not by assuming previously covered numeric ranges were complete.

## Review notes

| MR | Depth / weight | Result |
|---|---|---|
| !21190 | Scanned, merged stable backport | MPEG PMT extended-descriptor dissector-table backport of !21174; master MR carries the useful review discussion. |
| !21189 | Scanned, merged stable backport | MACsec distinguishes not-verified from verification-failed; backport of !21163. |
| !21188 | Deep/corroboration, merged master | Aethra adopts `ws_log`; discussion also reinforces putting mutable reader state in per-instance state for later passes. |
| !21187 | Scanned, merged master | K12 adopts project `ws_log` infrastructure; merged by Guy Harris. |
| !21186 | Scanned, merged stable backport | NSIS certification-page packaging/UI change; no durable engineering convention extracted. |
| !21185 | Scanned, merged stable backport | NAS 5GS multiple-payload cursor/length-base correction; sibling of !21181. |
| !21184 | Scanned, merged stable backport | NAS 5GS multiple-payload cursor/length-base correction; sibling of !21181. |
| !21183 | Scanned, merged stable backport | NAS 5GS multiple-payload cursor/length-base correction; sibling of !21181. |
| !21182 | Scanned, merged stable backport | ORAN preference-value semantic correction; backport of !21173. |
| !21181 | Deep/corroboration, merged master | Corrects the base used for an explicitly length-delimited NAS payload entry; reinforces keeping offsets and declared lengths in one coordinate system. |
| !21180 | Scanned, merged master | NSIS certification-page change; no durable engineering convention extracted. |
| !21179 | Deep/promoted, merged master | Constifies non-mutating wmem container receiver parameters while preserving intentionally mutable returned elements. Added to `api-domain-conventions.md`. |
| !21178 | Scanned, merged stable backport | IEEE 802.11 MLO key/PN endian correction; backport evidence. |
| !21177 | Scanned, merged stable backport | IEEE 802.11 endian/warning fixes; backport evidence. |
| !21176 | Scanned, merged stable backport | DECT NR extension-table work; backport of !21170. |
| !21175 | Scanned, merged master | IEEE1905 explicit item encodings; useful local cleanup but no new general rule. |
| !21174 | Deep/promoted corroboration, merged master | John Thacker explicitly recommends passing an exact subset TVB to the nested MPEG descriptor dissector instead of carrying outer tag/length framing. Added as direct evidence to `parser-boundary-conventions.md`. |
| !21173 | Scanned, merged master | ORAN udCompHdr preference-semantic correction; no distinct new general rule. |
| !21172 | Scanned, merged master | Exposes capture-filter optimizer control in Qt and documents it; feature/UI work. |
| !21171 | Scanned, merged master | Aethra allocation lifetime/memory-leak repair; reinforces existing ownership guidance. |
| !21170 | Deep/corroboration, merged master | Uses dissector tables for DECT NR MAC/IE extension points and adds context checks; reinforces extension-point dispatch architecture. |
| !21169 | Scanned, merged master | IEEE 802.11 endian warnings/corrections; protocol-specific correctness. |
| !21168 | Scanned, merged master | Corrects little-endian interpretation of MLO IGTK/BIGTK IDs and packet numbers. |
| !21167 | Deep/promoted, merged master | Consolidates near-duplicate log3gpp record-building paths. Guy Harris explicitly endorses deduplication because divergent near-copies can be buggy; semantic changes whose intent is uncertain are deliberately kept separate. Added to `refactoring-conventions.md`. |
| !21166 | Scanned, merged master | Typo-only cleanup. |
| !21165 | Deep/corroboration, merged master | Fixes an off-by-one bounds predicate so a NetMon frame table may end exactly at EOF; reinforces precise half-open boundary reasoning. |
| !21164 | Scanned, merged master | Earlier log3gpp record-building deduplication step; supports !21167. |
| !21163 | Deep/corroboration, merged master | MACsec separates unchecked state from failed verification; reinforces semantic status-state distinctions. |
| !21162 | Scanned, merged master | Avoids passing an empty capture-filter argument; command-boundary correctness. |
| !21161 | Scanned, merged master | Displays unoptimized compiled capture filters consistently with capture configuration. |
| !21160 | Scanned, merged master | BIST-OUCH orderbook state tracking feature; substantial protocol feature but no broadly reusable convention promoted. |
| !21159 | Scanned, merged master | Corrects MKA acceptable-PN bit significance; protocol-specific semantic fix. |
| !21158 | Deep/corroboration, merged master | Buffer reservation now accounts for every emitted component rather than relying on allocator spare capacity; strongly reinforces existing exact-sizing guidance. |
| !21157 | Scanned, merged stable backport | DECT NR specification update backport. |
| !21156 | Scanned, merged master | DECT NR specification update; no new general engineering rule. |
| !21155 | Scanned, merged master | Constifies static wsutil data and moves it to read-only storage; useful cleanup, no distinct new rule beyond const-correctness. |
| !21154 | Scanned, merged master | Packaging follows temporarily disabled Falco plugins so CI does not seek unbuilt DLLs. |
| !21153 | Scanned, merged master | Temporary CI disablement of failing Falco plugins; operational workaround, not architectural precedent. |
| !21152 | Deep but down-weighted, open/unresolved | CONNECT-IP over HTTP/3 explores nested synthetic frame/packet state and has extensive discussion/sample requests, but remains unresolved and therefore is not treated as accepted architectural precedent. |
| !21151 | Scanned, merged master | `.mailmap` maintenance only. |
| !21150 | Scanned, merged master | Debian setup dependency-install fix; tooling maintenance. |
| !21149 | Scanned, merged master | Propagates `--no-optimize` capture option to dumpcap; command-plumbing feature. |
| !21148 | Scanned, merged automatic update | Generated registry/translation maintenance. |
| !21147 | Scanned, merged automatic update | Generated registry/translation maintenance. |
| !21146 | Scanned, merged automatic update | Generated registry/translation maintenance. |
| !21145 | Scanned, merged automatic update | Generated registry/translation maintenance. |
| !21144 | Scanned, merged master | Additional dissector warning cleanup; reinforces checker-clean code but no new convention. |
| !21143 | Scanned, merged stable/backport sibling | HTTP/2 existing-stream window update correction; sibling of !21141/!21142. |
| !21142 | Deep/corroboration, merged sibling | HTTP/2 computes the settings delta from the old initial-window value before storing the new value; reinforces consume-old-state-before-mutation reasoning. |
| !21141 | Scanned, merged sibling | HTTP/2 existing-stream window update correction; same fix family as !21142/!21143. |

## Durable notebook changes

- Added `refactoring-conventions.md` with the Guy Harris-supported rule to centralize duplicate/near-duplicate parser paths while keeping uncertain semantic changes separate.
- Extended `parser-boundary-conventions.md` with !21174 as direct John Thacker review evidence for exact subset-TVBuff interfaces at nested framing boundaries.
- Extended `api-domain-conventions.md` with !21179's const-correct read-only container receiver convention.

Strong findings such as !21158's exact buffer sizing, !21165's precise EOF bounds condition, !21188's per-instance reader state, and !21163's unchecked-versus-failed status distinction were retained as corroboration of conventions already present in the notebook rather than creating overlapping rules.
