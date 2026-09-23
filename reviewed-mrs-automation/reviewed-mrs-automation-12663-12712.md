# Automated Wireshark MR review: !12663 through !12712

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`

Notebook repository: `dheitmueller/wireshark-chatgpt`

## Selection and tracking

The already-reviewed set was reconstructed from all available review tracking in the notebook, including `reviewed-mrs.md`, the aggregate automation tracker where applicable, and the available files under `reviewed-mrs-automation/`. Selection was performed by MR number after subtracting that explicit reviewed set; no numeric interval was assumed to be reviewed merely because neighboring MRs appeared in a ledger.

The historical !17571-!17620 batch remains preserved and counted as previously reviewed. The preceding run's inspection of !12712 was only a frontier probe and was not counted as a review, so !12712 correctly begins this batch.

Exactly 50 MRs were reviewed in this run: !12712 through !12663 inclusive. Of these, 46 are merged and 4 are closed/unmerged (!12707, !12696, !12687, and !12686). Closed and superseded work was down-weighted relative to merged changes.

## Exact reviewed-MR ledger

| MR | Status / weight | Review result |
| --- | --- | --- |
| !12712 | merged | BLF Ethernet-status cleanup; localized duplicate-tree-item/label correctness fix, no new durable convention. |
| !12711 | merged, maintainer-authored | Martin Mathieson makes USB-COM helpers file-local/static; straightforward encapsulation cleanup. |
| !12710 | merged, high weight | John Thacker removes VNC connection globals. Negotiated depth/bytes-per-pixel move to conversation state, while packet-specific historical values are snapshotted for deterministic redissection. Strong corroboration of existing dissector-state/first-pass rules. |
| !12709 | merged | TLS Token Binding visualization/support with sample coverage; useful protocol feature evidence, no distinct general rule. |
| !12708 | merged, high weight | John Thacker fixes an NTP fixed-point edge case: raw all-zero means unavailable, but lossy conversion could turn the smallest nonzero fraction into zero. Promoted to `wire-encoding-sentinel-conventions.md`: classify wire sentinels before lossy conversion. |
| !12707 | closed/unmerged draft, down-weighted | Follow-stream performance experiment required rebase/additional work and was not accepted; retained only as context. |
| !12706 | merged | Global-profile handling improvement; localized profile/UI behavior, no new general convention. |
| !12705 | merged | Falco lookup/fallback robustness fix; localized defensive lookup behavior. |
| !12704 | merged | BLF APP_TEXT metadata segmentation handling; protocol/file-format specific. |
| !12703 | merged | Display-filter time multiplication/division support; accepted feature work without a distinct new review convention. |
| !12702 | merged backport | Stable-branch backport of Nettrace timestamp fix; corroborates master behavior. |
| !12701 | merged | Nettrace timestamp parsing/correction; localized wiretap fix. |
| !12700 | merged | Spelling/documentation cleanup only. |
| !12699 | merged | JA4/TLS ALPN formatting for non-printable values; specification/output correctness, no broader rule added. |
| !12698 | merged backport | DHCP option 120 tree item uses the tvbuff matching the offset coordinate system. Strongly corroborates existing tvbuff-coordinate guidance. |
| !12697 | merged backport | Same DHCP option 120 coordinate-space fix on another stable branch. |
| !12696 | closed/unmerged, down-weighted | Samba/pidl regeneration proposal failed CI and was not accepted; generated-code evidence is weaker than merged source-of-truth examples. |
| !12695 | merged backport | DHCP option 120 coordinate-space fix for release-4.2; reinforces the same accepted rule. |
| !12694 | merged | vcpkg export packaging backport; localized build/package maintenance. |
| !12693 | merged | vcpkg packaging/fixup adjustment; localized. |
| !12692 | merged | Samba SAMR IDL synchronization/regeneration; corroborates keeping generated output aligned with authoritative generator inputs. |
| !12691 | merged | Lua `PInfo.port_type` write support brings implementation into line with documented API behavior and tests. |
| !12690 | merged | BLF APP_TEXT handling removes unnecessary NUL construction and fixes cleanup; localized memory/text handling. |
| !12689 | merged, maintainer-authored | John Thacker updates macOS libgcrypt and aligns dependency versions with other supported build environments; packaging consistency evidence. |
| !12688 | merged, maintainer-authored | Martin Mathieson fixes RedCap UL CCCH handling in MAC-NR; protocol-specific correctness. |
| !12687 | closed/unmerged, down-weighted | ISAKMP/RFC update did not reach an accepted state; pipeline/rebase and completeness issues remain, so not used as normative evidence. |
| !12686 | closed/unmerged, down-weighted | Nettrace session-level parsing proposal missed the possibility of multiple `<msg>` elements per session; Anders Broman flagged the structural issue and later closed the MR as no longer needed. Useful negative evidence only. |
| !12685 | merged, maintainer-authored | John Thacker advances openSUSE CI images/builds to 15.5; routine CI maintenance. |
| !12684 | merged, maintainer-authored | John Thacker makes macOS setup reject dependency versions below Wireshark's supported minima; build-script consistency. |
| !12683 | merged, maintainer-authored | John Thacker raises the macOS minimum implied by Qt 5.10. Guy Harris caught an incorrect supporting commit reference in the MR description, which was corrected before merge; useful submission-review hygiene but too narrow for a separate rule. |
| !12682 | merged backport | GnuTLS FIPS-lax handling backported to release-3.6. |
| !12681 | merged backport | GnuTLS FIPS-lax handling backported to release-4.0. |
| !12680 | merged backport | GnuTLS FIPS-lax handling backported to release-4.2. |
| !12679 | merged, maintainer-authored | John Thacker adds GnuTLS FIPS-lax handling on master; accepted platform/crypto policy fix. |
| !12678 | merged | Master DHCP option 120 coordinate-space fix; reinforces existing tvbuff-coordinate convention. |
| !12677 | merged backport | GitLab CI logs CI-prefixed environment variables to improve diagnosability; backport corroboration. |
| !12676 | merged | GitLab CI logs CI-prefixed environment variables in collapsed sections for debugging; corroborates existing CI observability guidance. |
| !12675 | merged | Transifex API v3 migration. Discussion exposed the separate weekly-update automation and led to dry-run support before bulk updates; useful operational testing evidence without a new standalone convention. |
| !12674 | merged backport | VMware heartbeat display-filter abbreviation correction; stable API/filter-name correctness. |
| !12673 | merged backport | VMware ESXi build-number table update for release-4.2; data maintenance. |
| !12672 | merged | Wireshark 4.2.0rc2 release-build metadata/notes update; release engineering, no new convention. |
| !12671 | merged backport | Windows nghttp3 1.0.0 dependency update on release-4.2; packaging maintenance. |
| !12670 | merged backport | IEEE 1609.2 ASN.1 cleanup uses original standard files with comments and only necessary generator-order adjustments, with links to originals; strong corroboration of source-of-truth/generated-code practice. |
| !12669 | merged backport | Nettrace support for both NAS-EPS and NAS-5GS; protocol-specific parsing enhancement. |
| !12668 | merged backport | Nettrace parser fixes an offset advanced twice; stable-branch corroboration of master fix. |
| !12667 | merged backport | PFCP TEID is four bytes, not one, and field type/label are corrected accordingly; stable-branch protocol-layout fix. |
| !12666 | merged | Master PFCP TEID width/type correction; concrete field-width correctness, no broader new rule. |
| !12665 | merged | Master Nettrace offset-advance fix; localized parser cursor correction. |
| !12664 | merged, high-confidence accepted revert | João Valverde reverts root-level plugin discovery / `plugin_type` changes because the usability simplification introduced other issues without an easy robust fix. Important architecture signal, but insufficient detail here to promote a more specific plugin rule beyond preserving the established typed subfolder model. |
| !12663 | merged | VMware ESXi build-number table update on master; data maintenance. |

## Durable notebook update from this run

- Added `wire-encoding-sentinel-conventions.md` from merged !12708. Protocol-defined special encodings must be recognized in the native wire representation before rounding, scaling, narrowing, or other lossy conversion can collapse nearby ordinary values onto the sentinel. Review should include boundary values such as the smallest nonzero representable timestamp when zero is special.

## Strong corroborating evidence retained without duplicate rules

- !12710 independently reinforces existing guidance to keep negotiated state in the appropriate conversation object, mutate evolving state on the first pass, and persist packet-specific historical values for redissection rather than depending on mutable globals.
- !12698, !12697, !12695, and !12678 reinforce that a protocol-tree field's offset must be interpreted in the coordinate space of the tvbuff passed to the add-item API; a composite tvbuff cannot be substituted merely because it contains equivalent bytes.
- !12692 and !12670 reinforce keeping generated dissector artifacts traceable to authoritative generator/source inputs rather than hand-maintaining diverged generated code.
- !12676/!12677 and the !12675 discussion reinforce making CI/maintenance automation diagnosable and providing safe validation/dry-run paths for bulk automated changes.
- Guy Harris's review on !12683 was given appropriate weight: supporting commit references in MR descriptions must actually substantiate the statement being made. Because this was a one-line correction rather than a recurring architectural pattern, it is retained here rather than promoted to a standalone convention.

## Down-weighted evidence

The four closed/unmerged MRs were reviewed but not treated as accepted design:

- !12707: stale/unmerged performance experiment.
- !12696: unsuccessful generated-code regeneration with CI failure.
- !12687: incomplete/unmerged protocol update with pipeline/rebase issues.
- !12686: parser redesign whose structure was questioned in review and which was subsequently closed as unnecessary.

## Frontier check

MR !12662 (`macos-setup: use compile options for libpcre and zstd. [skip ci]`) exists in corpus commit `ddcaa22b51c68f594e425a23388c3a2086813054` and is merged. It was inspected only to prove that the corpus continues below this batch and is **not** counted as reviewed here. Absent newly scraped higher-numbered unreviewed MRs, !12662 is the next descending candidate.
