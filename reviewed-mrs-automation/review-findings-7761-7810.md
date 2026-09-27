# Review findings: Wireshark !7761-!7810

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

All 50 MRs in this batch are merged.

| MR | Outcome | Depth | Finding |
|---|---|---|---|
| !7810 | merged | Scanned | macOS CI moves package jobs to Qt 6.2.4. |
| !7809 | merged | Scanned | Bluetooth 5.3 backport. |
| !7808 | merged | Scanned | Adds Bluetooth 5.3 LL version decoding. |
| !7807 | merged | Scanned / backport | BTATT battery-power-state bitmask correction. |
| !7806 | merged | Scanned / backport | Same BTATT mask correction on another branch. |
| !7805 | merged | Scanned / backport | Same BTATT mask correction on another branch. |
| !7804 | merged | Scanned | Corrects the four two-bit BTATT battery-power-state masks. |
| !7803 | merged | Deep / corroboration | RDPUDP retained server address switches to file-scope wmem ownership. |
| !7802 | merged | Discussion-focused | EVPN ADD-PATH uses a heuristic whose possible legal collision Uli Heilmeier identified; accepted protocol-specific compromise. |
| !7801 | merged | Scanned | SMB Export Objects frees a removed free-chunk node. |
| !7800 | merged | Deep | John Thacker adds an ExportObjectModel destructor that frees owned export entries. |
| !7799 | merged | Deep / promoted | DLEP bounds each data item with a subset TVB, dispatches through an extensible table, localizes bounds failures, and preserves sibling synchronization. |
| !7798 | merged | Deep / Guy | Guy Harris changes Ascend parser timestamp state from guint32 to time_t, matching mktime() and downstream use. |
| !7797 | merged | Scanned | Adds an independent Logray patch-version variable. |
