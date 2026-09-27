# Wireshark MR review automation ledger: !7261–!7310

Corpus repository: `dheitmueller/wireshark-corpus-mrs`  
Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`  
Notebook base: `298a800596718b487c8d08cc0413072a6669722a`  
Review direction: descending MR number (newest available unreviewed first)

## Selection reconciliation

Before selecting this batch, review tracking on the notebook base was reconciled from `reviewed-mrs.md`, the per-run ledgers under `reviewed-mrs-automation/`, and all irregular exact-list/gap/backfill/noncontiguous ledgers present in that tree. None records !7261–!7310 as reviewed. The preceding !7311–!7360 ledger mentions !7310 only in its **Next frontier** section and explicitly says it was not counted as reviewed.

The historical `reviewed-mrs-automation/reviewed-mrs-automation-17571-17620.md` ledger was also revalidated: it contains all 50 unique MR numbers !17571 through !17620. That batch remains preserved and counted.

## Exact reviewed set

| MR | State | Title |
|---:|:---|:---|
| !7310 | merged | SOME/IP: Make uats much more robust against faulty configs (BUGFIX) |
| !7309 | merged | Qt: Column edit default checkbox disabled |
| !7308 | merged | Rename Logwolf to Logray |
| !7307 | merged | gtp: Fix copy-paste error |
| !7306 | merged | Locamation Interface Module dissector for IM1 |
| !7305 | merged | dcp-etsi: Strengthen heuristic, add for Decode As |
| !7304 | merged | STUN: Check the Fingerprint (CRC32) |
| !7303 | merged | knxip: Add a port range preference |
| !7302 | merged | tplink-smarthome: Add a brief heuristic |
| !7301 | merged | Tools: Remove fixhf.pl. |
| !7300 | merged | packet-gtp.c: Fix copy-paste error (Coverity 1506627) |
| !7299 | merged | register.c: Avoid potential race condition (Coverity 1477510) |
| !7298 | merged | Qt: Cleanup PacketListHeader |
| !7297 | merged | Qt: Reduce PacketListHeader complexity |
| !7296 | merged | Qt: Improve sort for packet list |
| !7295 | merged | Ui: Centralize PacketList helper prototypes |
| !7294 | merged | Ui: Use only one method for exit |
| !7293 | merged | ReleaseNotes: Correct some spellings and wordings |
| !7292 | merged | STUN: Set conversation dissector after any STUN packet |
| !7291 | merged | Qt: Add resolved button to Edit Columns |
| !7290 | merged | tools: Port make-sminmpec.pl to make-sminmpec.py |
| !7289 | merged | file: Fix documentation |
| !7288 | merged | Qt: Fix FileClose not available and segfault |
| !7287 | merged | conversation_dialog.h: Fix -Wdocumentation |
| !7286 | merged | Make some variables in packet-grebonding.c static. |
| !7285 | merged | Ui: Further simplify ws_ui_util |
| !7284 | merged | Ui: Remove time column reformat callback |
| !7283 | merged | Ui: Remove call to recoloring |
| !7282 | merged | Ui: Remove call to recoloring |
| !7281 | merged | Ui: Remove unused prototype declaration |
| !7280 | merged | Qt: Better handle sort restriction |
| !7279 | merged | tools: Port colorfilters2js.pl to colorfilters2js.py |
| !7278 | merged | Qt: Make the Resolve Names buttons checkable again |
| !7277 | merged | Qt: Make the Resolve Names buttons checkable again |
| !7276 | merged | Qt: Make the Resolve Names buttons checkable again |
| !7275 | closed | Draft: EAP: don't assume g_strsplit_set() will produce enough tokens. |
| !7274 | merged | Version: 3.7.1 → 3.7.2 |
| !7273 | merged | wsdg/Lua: no get_range() method - use fieldinfo.range |
| !7272 | merged | Build: 3.7.1 |
| !7271 | closed | dfilter: fix compile error may be used uninitialized in this function |
| !7270 | merged | Qt: Increase animation speed for progress frame |
| !7269 | merged | stun: Tighten heuristic by rejecting restricted values |
| !7268 | merged | DoIP: Support UAT for User defined payload types |
| !7267 | merged | dfilter: Fix undefined dereference and add null check |
| !7266 | closed | epan: Add a NULL check. |
| !7265 | merged | STUN: Update some comments |
| !7264 | merged | Minor Python3 script fixups. |
| !7263 | merged | [Automatic update for 2022-06-26] |
| !7262 | merged | [Automatic update for 2022-06-26] |
| !7261 | merged | [Automatic update for 2022-06-26] |

Count: **50 unique MRs**.  
State weighting: **47 merged**, **3 closed/unmerged** (!7275, !7271, !7266).

## Weighting notes

Merged MRs are primary implementation evidence. Closed !7275, !7271, and !7266 are retained for review/history only and are not treated as accepted implementation precedent. In particular, !7266 is valuable negative evidence because the proposed NULL-check workaround was closed after Roland Knall identified the actual fix as moving initialization to the point where the value becomes semantically available; merged !7267 is the accepted result.

Maintainer-authored or maintainer-reviewed material by Guy Harris, John Thacker, Gerald Combs, Alexis La Goutte, Roland Knall, João Valverde, Martin Mathieson, and others is weighted according to specificity and acceptance status.
