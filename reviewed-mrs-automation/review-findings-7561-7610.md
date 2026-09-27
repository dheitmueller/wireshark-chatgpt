# Review findings: !7561-!7610

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged work is primary evidence; closed !7610 is review reasoning only.

| MR | Outcome | Depth | Finding |
|---|---|---|---|
| !7610 | closed | Discussion | John Thacker: initialize TCP base sequence state before sequence analysis; analyze only when segment length is known; apply relative-number presentation afterward. |
| !7609 | merged | Scan | ISUP parameter summary duplication fix. |
| !7608 | merged | Scan | MySQL old/new Authentication Method Switch layouts separated. |
| !7607 | merged | Scan | GTP release ordering and RADIUS length accounting fixed. |
| !7606 | merged | Discussion | MySQL capability update accompanied by focused zlib/zstd capture. |
| !7605 | merged | Scan | CIP UTIME microsecond remainder converted correctly to nstime nanoseconds. |
| !7604 | merged | Scan | ISUP tap carries variant so consumers use the right message table. |
| !7603 | merged | Scan | Protobuf generated-field byte ranges corrected. |
| !7602 | merged | Scan | Qt capitalization cleanup. |
| !7601 | merged | Deep | X.25 payload probing uses `next_tvb`; parent tvb plus old offset is not equivalent after reassembly. |
| !7600 | merged | Scan | About/version output refactor. |
| !7599 | merged | Scan | Ubuntu workflow path fix. |
| !7598 | merged | Discussion | Jaap Keuter pointed to an authoritative currency-code source; additions remained scoped to realistic ZVT use. |
| !7597 | merged | Scan | Couchbase avoids empty bitmask-tree field list. |
| !7596 | merged | Scan | Same Couchbase fix in counterpart branch. |
| !7595 | merged | Deep | Follow Stream lookup retrieves existing conversations only; UI/filter construction must not create streams or conversation state. |
| !7594 | merged | Scan | ZVT Maestro card type. |
| !7593 | merged | Scan | `_U_` annotations aligned with actual use and declarations. |
| !7592 | merged | Scan | Automatic update. |
| !7591 | merged | Scan | Automatic update. |
| !7590 | merged | Scan | Automatic update; Asterix generation failure noted. |
| !7589 | merged | Scan | Exit now stops extcap even if no packet has arrived. |
| !7588 | merged | Scan | About dialog tweak. |
| !7587 | merged | Deep | QUIC Follow Stream carries protocol-known server direction; first-seen endpoint inference fails for streams and migration. |
| !7586 | merged | Deep | John Thacker/Roland Knall: endpoint/conversation UI capabilities must derive from actually registered table fields, not protocol-name guesses. |
| !7585 | merged | Discussion | Roland Knall redirected the fix toward missing signal/slot wiring instead of direct underlying-class calls. |
| !7584 | merged | Deep | Capture pipe handling moved from separate UI implementations into shared GLib-mainloop capture code. |
| !7583 | merged | Deep | John Thacker: one-bit `gboolean` bitfields can promote true to -1; use standard `bool` for boolean bitfields. |
| !7582 | merged | Scan | Aeron static-analysis warning cleanup. |
| !7581 | merged | Deep | RTP payload-type preferences converted to automatic dissector-table preferences, eliminating duplicated callback/range state. |
| !7580 | merged | Deep | Legacy preference migration distinguishes protocol filter/module names from dissector short names; the identifier domains are not interchangeable. |
| !7579 | merged | Scan | Visual Studio path fix. |
| !7578 | merged | Scan | extcap docs fixups. |
| !7577 | merged | Scan | User-guide link addition. |
| !7576 | merged | Scan | Dependency update reverted after an incompatible license change; version freshness is not the only upgrade criterion. |
| !7575 | merged | Scan | Man-page dependency backport. |
| !7574 | merged | Scan | Reproducible-docs footer removal backport. |
| !7573 | merged | Scan | Same reproducible-docs backport. |
| !7572 | merged | Scan | Man-page dependencies modeled through `add_custom_command`. |
| !7571 | merged | Scan | Removes source-mtime-derived HTML footer for reproducible output. |
| !7570 | merged | Scan | Version correction. |
| !7569 | merged | Scan | Revert of prior Qt cleanup. |
| !7568 | merged | Discussion | ISAKMP algorithm/attribute support; review centered on CI/account mechanics. |
| !7567 | merged | Deep | TCP tap uses cleanup handling so valid fixed-header metadata is delivered even when later option parsing throws, exactly once. |
| !7566 | merged | Scan | packet_info comment correction. |
| !7565 | merged | Scan | Removes obsolete EPEL packaging special case. |
| !7564 | merged | Deep | SCCP handles producer-side external reassembly past nominal DATA length only via an explicit preference plus expert assumption guidance. |
| !7563 | merged | Scan | Version bookkeeping. |
| !7562 | merged | Discussion | Python Match API choice was checked against documentation after Gerald Combs questioned it. |
| !7561 | merged | Scan | Capture-file dialog defaults to relevant capture files. |

Count: **50 rows / 50 unique MR numbers**.
