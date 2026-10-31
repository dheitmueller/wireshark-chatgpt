# Automated Wireshark MR review — !9662 through !9613

Review date: 2026-10-30
Corpus repository: `dheitmueller/wireshark-corpus-mrs`
**Corpus commit:** `ddcaa22b51c68f594e425a23388c3a2086813054`
Notebook starting commit: `3be4ef40b8f8f45dd46c417d737cf835389ada75`
Selection: descending numeric MR order, highest available previously unreviewed first.

**Exact batch: 50 distinct MRs, 49 merged and one closed/unmerged (!9658).**

## Selection, evidence and duplicate avoidance

Before selecting, consulted `reviewed-mrs.md`, the supplemental `reviewed-mrs-automation/reviewed-mrs-automation.md`, adjacent run ledgers `reviewed-mrs-automation-9663-9712.md` and `reviewed-mrs-automation-9713-9762.md`, and repository-wide exact-MR-number searches for **each of the 50 candidates**. The sole candidate-number search hit was !9662 in the previous ledger's next-expected-candidate context, not a reviewed-membership row. The previous ledger explicitly covers !9663–!9712. No numeric interval was presumed reviewed just because part of it appeared in tracking. The corpus main head was verified unchanged at the commit above and all fifty selected `mr_N.json` records were fetched; their retained final `changes_data` and non-system `discussions_data` were inspected, with deeper attention to significant diffs and maintainer discussion.

The preserved `reviewed-mrs-automation-17571-17620.md` was separately fetched and its **50 distinct explicit membership rows** checked; it was not modified. Merged master code and explicit maintainer feedback (John Thacker, Guy Harris, João Valverde, Gerald Combs, Alexis La Goutte) received higher weight than automatic release backports or superseded submissions. A merged MR's post-merge review can still identify a regression; the closed !9658 is reviewer guidance only, not an accepted implementation. The corpus omitted the large Windows Delivery Optimization source-file patch, so no detailed implementation claim depends on that diff.

These findings are a repository review, not a fresh Wireshark build, test, or fuzzing run.

## Exact reviewed-MR membership (descending)

| MR | Corpus outcome | Depth | Evidence and finding |
| --- | --- | --- | --- |
| !9662 | merged | Deep | Gerald's initial macOS CI/Homebrew job; post-merge Chuck/John found required speexdsp missing, addressed via !9679. |
| !9661 | merged | Scanned | RTPS PING string dissection; small protocol-specific field expansion. |
| !9660 | merged | Scanned | NGAP NAS PDU IE regression fixed in ASN.1 .cnf source and generated packet-ngap.c. |
| !9659 | merged | Scanned | NAS 5GS network-feature-support IE decode correction. |
| !9658 | closed/unmerged | Discussion-focused | Closed/unmerged Windows Delivery Optimization dissector, superseded by !10124; John recommends expert info, Decode As-friendly match_uint, and correct protocol label; Alexis requires topic branch and release notes. Large source-file diff unavailable. |
| !9657 | merged | Scanned | Make PFCP/SAP diagnostics helper symbols file-local (static). |
| !9656 | merged | Scanned | Logray Qt 6.4 build fix for deleted Qt conversion overload. |
| !9655 | merged | Deep | Widespread field mask/width cleanup; John identifies ASN.1 template source of generated-file mismatch and points to !9892. |
| !9654 | merged | Deep | RTPS DomainId must come from discovery rather than deducing from non-default port numbers. |
| !9653 | merged | Deep | João's dependency pruning; John flags plugin ABI/link contract; final merged revision retains wsutil public for epan/wiretap and isolates other dependencies. |
| !9652 | merged | Scanned | Register TLS for HTTP Upgrade subdissector dispatch per RFC 2817. |
| !9651 | merged | Scanned | MSYS2 README maintenance. |
| !9650 | merged | Scanned | release-3.6 backport of CMake documentation filename case fix; corroborates !9648. |
| !9649 | merged | Scanned | release-4.0 backport of CMake documentation filename case fix; corroborates !9648. |
| !9648 | merged | Scanned | Master CMake source list lowercases gpl_appendix.adoc after documentation rename. |
| !9647 | merged | Scanned | Revert incomplete Linux rpath/relocation change; no positive relocation implementation to generalize. |
| !9646 | merged | Scanned | release-3.6 documentation filename preparation backport of master work. |
| !9645 | merged | Scanned | release-4.0 documentation filename preparation backport of master work. |
| !9644 | merged | Scanned | release-3.6 macOS DMG change copies top-level .pkg rather than symlinking it. |
| !9643 | merged | Scanned | release-4.0 macOS DMG change copies top-level .pkg rather than symlinking it. |
| !9642 | merged | Scanned | release-3.6 NAS 5GS CAG-only flag text ordering correction. |
| !9641 | merged | Scanned | release-4.0 NAS 5GS CAG-only flag text ordering correction. |
| !9640 | merged | Scanned | Master NAS 5GS CAG-only flag text ordering correction per 3GPP TS 24.501. |
| !9639 | merged | Scanned | Correct IPv6 MTU option comment typo with RFC 9268 reference. |
| !9638 | merged | Deep | John Thacker's RTP multiplexing preference adopts RFC 7983 classification for nonheuristic dispatch, preserving separate heuristic ownership. |
| !9637 | merged | Discussion-focused | GREBonding link-value fix; Martin notes duplicate value_string key 3 detected by registration checker. |
| !9636 | merged | Deep | Typed-item API checker prompted width/mask corrections and narrowly scoped warning handling in many dissectors. |
| !9635 | merged | Scanned | macOS codesign uses org.wireshark prefix for utility and library identifiers. |
| !9634 | merged | Scanned | ChmodBPF postinstall uses current launchctl bootstrap system syntax; removes legacy StartupItems assumption. |
| !9633 | merged | Scanned | Master developer/user guide sources renamed consistently and references updated. |
| !9632 | merged | Scanned | MSYS2 setup package maintenance. |
| !9631 | merged | Deep | Fix too-short proto_tree item spans across several dissectors to match registered widths. |
| !9630 | merged | Scanned | Master macOS DMG packaging copies .pkg instead of symlinking; release backports !9643/!9644. |
| !9629 | merged | Deep | Checker reduces false positives with targeted regex ignores and repairs genuine duplicate/incorrect filter IDs. |
| !9628 | merged | Scanned | Automated master protocol and registry data refresh, no independent human-review lesson. |
| !9627 | merged | Scanned | Automated release-4.0 registry data refresh; maintenance backport. |
| !9626 | merged | Scanned | Automated release-3.6 registry data refresh; maintenance backport. |
| !9625 | merged | Deep | Extcap distinguishes any exposed settings from unresolved mandatory settings with defaults; Gerald requests explicit UTF-8 conversion; Tomasz catches Boolean-widget regression. |
| !9624 | merged | Scanned | Packet-list column caching during color dissection allowed only when it cannot evict existing cached data. |
| !9623 | merged | Scanned | Remove MSYS2 packages now available upstream, update PKGBUILD and README. |
| !9622 | merged | Deep | Dependent-frame list replaced with hash set; John catches NULL-table foreach assertion under lazy initialization. |
| !9621 | merged | Scanned | Filesystem path routines centralize environment-variable precedence for data/plugin directories. |
| !9620 | merged | Deep | VITA 49 CIF1 field extension and sample capture; Alexis catches FT_BOOLEAN/item-width mismatch. |
| !9619 | merged | Discussion-focused | RDP uncompressed ZGFX/EGFX fixes; Alexis requests clang dead-store warning cleanup and discusses backport. |
| !9618 | merged | Scanned | NFSv4.1 CB_RECALL_ANY operation per RFC 5661. |
| !9617 | merged | Deep | Dissector generator gains plugin skeleton, plugin CMake setup, registration, install, and CHECKAPI wiring. |
| !9616 | merged | Deep | HTTP uses pinfo->pool and packet protocol data instead of ambient packet scope/global state. |
| !9615 | merged | Discussion-focused | ICMPv6 EARO feature; Alexis requests sample and author supplies encrypted pcap plus decryption preference instructions. |
| !9614 | merged | Discussion-focused | Couchbase snapshot bits; Alexis requests pcap exercising new flags and approves attaching it to MR. |
| !9613 | merged | Deep | Guy Harris-authored man-page cleanup clarifies UNIX-compatible platforms, macOS app-bundle paths, preferences and synchronized CLI docs. |

## Durable notebook changes

- `build-conventions.md`: dependency visibility versus plugin contract (!9653/!9617), CI platform bootstrap dependency checks (!9662).
- `dissector-conventions.md` and `dissector-table-registration-conventions.md`: multiplexed/heuristic dispatch (!9638), RTPS discovered identity (!9654), reviewer advice on Decode As matched ports and packet expert diagnostics (!9658).
- `dissector-state-conventions.md` and `memory-management-conventions.md`: packet-owned HTTP state and allocation (!9616); set-like frame dependencies and NULL-safe lazy tables (!9622).
- `field-registration-conventions.md`: checker-guided width/field corrections (!9655/!9636/!9631/!9629), VITA 49 Boolean width and duplicated enumerations (!9620/!9637).
- `submission-conventions.md`: topic branches and closed-resubmission evidence (!9658), reproducible encrypted sample captures (!9615/!9614), post-merge follow-ups.
- New `extcap-configuration-conventions.md`: defaulted mandatory arguments vs the presence of settings, and checkbox UI regression from !9625.
- New `documentation-conventions.md`: Guy Harris's cross-platform man-page accuracy (!9613), documentation/CMake filename alignment (!9633/!9648 and backports).

**Next expected candidate:** !9612, subject to rescanning the corpus and all tracking for newly added higher-numbered records.
