# Review findings: Wireshark MRs 1560-1609

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged work is stronger evidence than closed/superseded work. Maintainer-authored and maintainer-reviewed guidance is weighted accordingly.

| MR | Outcome | Finding |
|---|---|---|
| 1609 | merged | Guy Harris backport: choose CMake versions from actual macOS/Apple-Silicon support boundaries. |
| 1608 | merged | F1AP 16.4.0 updates ASN.1/CNF/template inputs and regenerated output together. |
| 1607 | merged | E1AP 16.4.0 generated-source update. |
| 1606 | merged | XnAP 16.4.0 generated-source update. |
| 1605 | merged | NGAP 16.4.0 generated-source update. |
| 1604 | merged | Gerald Combs release-note markup/protocol-list correction. |
| 1603 | merged | X2AP 16.4.0 generated-source update. |
| 1602 | merged | S1AP 16.4.0 generated-source update. |
| 1601 | merged | ISUP user-guide text accepted by Anders Broman. |
| 1600 | merged | Removes stale RTSP documentation placeholder. |
| 1599 | merged | LTE-RRC duplicate filter names fixed in conformance input and generated C. |
| 1598 | merged | Osmux user-guide text accepted by Anders Broman. |
| 1597 | merged | H.225 user-guide text accepted by Anders Broman. |
| 1596 | merged | Moshe Kaplan sharpened docs to state exactly what the counter shows and what users can do. |
| 1595 | merged | Stable Qt fix reads the real child scrollbar position for auto-scroll state. |
| 1594 | merged | WSDG basic-dissector instructions clarified. |
| 1593 | merged | SIP Flows and VoIP Calls dialogs get independent singleton state. |
| 1592 | merged | CAN heuristic work was split to improve reviewability; final code supports regular CAN IDs too. |
| 1591 | merged | MTP3 user-guide description accepted. |
| 1590 | merged | GSM statistics documentation accepted. |
| 1589 | merged | UAT gains 64-bit numeric parsing/validation and matching public-symbol bookkeeping. |
| 1588 | merged | Pascal Quantin required explicit enclosing-context state before a shared ASN.1 handler mutates DRB mapping state; also caught `&=` where setting a flag required `|=`. |
| 1587 | merged | Stable TPNCP fix for messages without CID. |
| 1586 | merged | John Thacker Apple-Silicon CMake change was explicitly validated on ARM Mac hardware by Roland Knall. |
| 1585 | merged | DICOM upstream format changed; fix updates the generator and regenerates output. |
| 1584 | closed | Superseded DICOM attempt contained unrelated TPNCP changes. |
| 1583 | closed | Superseded DICOM attempt contained accidental files/raw inputs. |
| 1582 | merged | Guy Harris required S1G PHY identity to propagate through wiretap/radiotap/radio-info and challenged unverified radiotap bit assignments; Martin Mathieson and Alexis La Goutte also enforced checker/static-analysis cleanup. |
| 1581 | merged | Valgrind showed a leak fix introduced teardown use-after-free; author reproduced and iterated until lifecycle validation was clean. |
| 1580 | merged | RTP sequence analysis incorporates timestamp ordering and corrected cycle accounting. |
| 1579 | merged | Qt scrollbar page-step behavior proved platform-dependent; later work narrows the behavior. |
| 1578 | merged | Master version of OverlayScrollBar slider-position fix. |
| 1577 | merged | Master version of TPNCP missing-CID fix. |
| 1576 | merged | Automatic release-3.2 data/documentation refresh. |
| 1575 | merged | Automatic release-3.4 data/documentation refresh. |
| 1574 | merged | Automatic master data/documentation refresh. |
| 1573 | merged | RTP/VoIP dialogs validate capture-file state before dereference. |
| 1572 | merged | Stable TPNCP spelling correction split from larger backport. |
| 1571 | merged | Stable TPNCP data update split from larger backport. |
| 1570 | merged | Guy Harris: cherry-pick stable-branch changes individually so branch history says exactly what was carried. |
| 1569 | closed | Whitespace-only ELF cleanup; no accepted implementation precedent. |
| 1568 | merged | master-3.2 scrollbar action forwarding backport. |
| 1567 | merged | release-3.4 scrollbar action forwarding backport. |
| 1566 | merged | Overlay scrollbar forwards child `actionTriggered` so auto-scroll reacts to user actions. |
| 1565 | merged | PDCP-LTE configuration validation rejects malformed textual values with actionable errors at update time. |
| 1564 | merged | RTP Analysis reuses common calculation helper instead of duplicate formulas. |
| 1563 | merged | John Thacker documents precise macOS release where system tar gained xz support. |
| 1562 | merged | John Thacker raises GnuTLS minimum only to the floor all supported distributions satisfy and removes dead compatibility branches. |
| 1561 | merged | RTP Player centers after rescaling, fixing late-start waveform centering. |
| 1560 | merged | ASN.1 refresh includes DOP filter-abbreviation correction. |

## Durable promotions

Promote: individual-cherry-pick history for stable backports (!1570); complete shared PHY metadata propagation and standards provenance (!1582); explicit context state for shared generated handlers (!1588); full-lifecycle dynamic-analysis validation of memory fixes (!1581); support-matrix-driven dependency floors (!1562, !1586, !1609); and actionable configuration validation (!1565).

Generated-source findings in !1608-!1602, !1599, and !1585 corroborate existing source-of-truth guidance.
