# Review findings: Wireshark MRs 1610-1659

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged work is weighted most strongly. Closed/unmerged submissions are retained only for review guidance or historical context.

| MR | Outcome | Depth | Finding |
|---|---|---|---|
| 1659 | merged | Discussion-focused | DoIP resynchronization fix. Jaap Keuter asked that version-identification work be split from the bug fix so maintenance/backporting stays simple. |
| 1658 | merged | Scanned | Older-GCC Qt portability fix uses `qAbs` instead of an ambiguous global `abs` overload. |
| 1657 | merged | Scanned | QMessageBox parent must be supplied through the constructor; inherited `setParent()` can reset dialog window flags. |
| 1656 | merged | Scanned | Parallel/stable form of the same QMessageBox construction fix. |
| 1655 | merged | Discussion-focused | Gerald Combs and Guy Harris favored removing obsolete Gerrit/patch-file workflow text rather than preserving vestigial documentation. |
| 1654 | merged | Discussion-focused | Anders Broman requested reuse of an existing `true_false_string` from `tfs.h` instead of defining another equivalent pair. |
| 1653 | merged | Deep / high-authority | Guy Harris said not to disable `validate-clang-check.sh` for Windows-only ETW code; narrowly exclude the platform-only translation units that cannot compile on the checker host. Rebase before editing concurrently changed checker infrastructure. |
| 1652 | merged | Deep corroboration | Duplicate filter abbreviations are fixed in ASN.1 `.cnf` inputs as well as generated C, reinforcing generator-source-of-truth practice. |
| 1651 | merged | Deep | 802.11r decryption increment shipped with focused captures/tests, old-test reruns, and Valgrind checks while leaving broader roaming work for later MRs. |
| 1650 | merged | Scanned | Unknown 802.11 environment values use `BASE_SPECIAL_VALS` presentation rather than an artificial “Unknown” value-string entry. |
| 1649 | merged | Deep | Reestablishes separate Qt `captureFileClosing()` and `captureFileClosed()` phases: teardown/disconnect first, then controls/UI state after the file is gone. |
| 1648 | merged | Scanned | IMAP username extraction fixes the source byte range to match the actual token. |
| 1647 | merged | Scanned | Corrects error-output pointer indirection and ownership in PDCP-LTE key parsing. |
| 1646 | merged | Scanned | Adds TLS delegated-credentials extension support with a focused handler and sample capture. |
| 1645 | merged | Scanned | Automated release-branch registry/data refresh; no new convention. |
| 1644 | merged | Scanned | Automated release-branch registry/data refresh; no new convention. |
| 1643 | merged | Scanned | Automated master registry/translation/docs refresh; no new convention. |
| 1642 | closed | Discussion-focused | Proposed installer-local extcap README was rejected as another documentation copy. Review favored improving/reusing the authoritative extcap docs instead. |
| 1641 | merged | Deep | TShark keeps an explicit “no -T seen yet” sentinel, rejects repeated `-T` output modes, and applies the default only after parsing. |
| 1640 | closed | Discussion-focused | Automatic `SSLKEYLOGFILE` consumption was rejected in this form; Peter Wu identified repeated full rereads and objected to implicit environment-driven behavior. |
| 1639 | merged | Scanned | BGP RFC 9003 update removes an obsolete 128-byte communication cap and uses the actual declared length. |
| 1638 | merged | Scanned / high-authority | Guy Harris-authored documentation correction. |
| 1637 | merged | Scanned / high-authority | Parallel Guy Harris-authored documentation correction. |
| 1636 | merged | Scanned | rpmbuild follows CMake verbosity: quiet by default unless verbose makefiles are requested. |
| 1635 | merged | Scanned | Clean resubmission of the documentation fix after the earlier bad-history MR. |
| 1634 | merged | Deep / high-authority | Guy Harris rejected preference-controlled semantic decoding. Recognized standardized GOOSE Float32 data should be shown numerically; unsupported forms fall back to raw octets. |
| 1633 | merged | Scanned | PKIX timestamp query/response files are registered through BER syntax handlers in generator/template inputs. |
| 1632 | closed | Scanned | Documentation fix carried unrelated conversation/TCP changes from bad history; closed and resubmitted cleanly. |
| 1631 | closed | Discussion-focused | gRPC streaming reassembly regression test did not merge here; later related work was referenced. Useful test idea but not accepted evidence in this snapshot. |
| 1630 | merged | Scanned | NR-RRC mapping propagates DRB data only when a DRB ID is actually present. |
| 1629 | merged | Scanned | PDCP-NR fixes error-pointer indirection and avoids treating encrypted undeciphered user-plane data as IP. |
| 1628 | merged | Scanned | Distribution generation becomes an explicit dependency; shell script uses `set -e -u -o pipefail` and handles existing tarballs idempotently. |
| 1627 | merged | Scanned | macOS setup updates Qt installer-version logic and drops obsolete branches. |
| 1626 | merged | Scanned | NR-RRC v16.3 update changes authoritative ASN.1/CNF/template inputs and regenerated output together. |
| 1625 | merged | Scanned | User-guide screenshot refresh only. |
| 1624 | merged | Scanned | LTE-RRC v16.3 update keeps authoritative ASN.1 inputs and generated output synchronized. |
| 1623 | merged | Scanned | LPP v16.3 reorganization updates extraction tooling, ASN.1 inputs, templates, and generated output consistently. |
| 1622 | merged | Scanned | Telecom-dialog follow-up disables actions requiring a live capture after file close; corroborates the explicit close-state lifecycle. |
| 1621 | merged | Discussion-focused | Jaap Keuter asked that the same always-zero stats-table index pattern be fixed in the second affected function, not only one occurrence. |
| 1620 | merged | Discussion-focused | Generated-file progress reporting moves into CMake and uses relative paths; João Valverde suggested keeping script chatter behind a verbose mode. |
| 1619 | merged | Discussion-focused | Documentation review distinguished historical involvement from current protocol authority; the text was accepted as reasonable despite limited expert provenance. |
| 1618 | merged | Deep / high-authority | Shared `iso8601_to_nstime()` replaces duplicated parsing and is exported through wsutil. Guy Harris required docs to list both accepted separators neutrally. Public-symbol metadata was updated. |
| 1617 | merged | Discussion-focused | Peter Wu rejected generic feature documentation; Martin Mathieson supplied concrete explanation of what the RLC graph shows and its scope. |
| 1616 | merged | Scanned | Release-branch TECMP display extent corrected from 3 bytes to the 2 bytes actually parsed. |
| 1615 | merged | Scanned | Master TECMP display extent corrected from 3 bytes to 2. |
| 1614 | merged | Scanned | macOS setup selects Python installers by platform support, including first Apple-Silicon-capable releases. |
| 1613 | closed | Scanned | Temporary Wayback-Machine dependency source proposal was closed when upstream downloads returned. |
| 1612 | merged | Deep corroboration | Fixes duplicate display-filter abbreviations across generated ASN.1 and manual MessagePack fields. One abbreviation must not represent incompatible field semantics/types. |
| 1611 | merged | Deep | FTP Export Objects accepted a documented best-effort model because FTP-DATA cannot prove completeness. Review narrowed scope toward real RETR/STOR transfers and added explicit size-policy discussion. |
| 1610 | merged | Scanned / high-authority | Guy Harris-authored macOS setup logic chooses CMake versions by actual OS/deployment-target support rather than always installing newest. |

## Durable promotions

This run promotes five focused conventions:
- Qt capture-file closing versus closed lifecycle from 1649, corroborated by 1622.
- Narrow platform-specific exclusions for static/checker tooling from Guy Harris review on 1653.
- Semantic decoding versus raw-byte fallback from Guy Harris review on 1634.
- Singular CLI output-mode parsing from 1641.
- Export Object completeness/resource behavior from 1611.

Other findings primarily corroborate existing notebook rules for generated code, focused MR scope, public-symbol bookkeeping, representative captures/tests, documentation quality, and source-of-truth discipline.
