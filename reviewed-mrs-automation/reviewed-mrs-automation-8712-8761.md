# Automated Wireshark MR review: !8761–!8712

**Reviewed:** 2026-10-31 using GPT-6 (meets the >= GPT-5.6 Sol requirement).
**Corpus repository:** dheitmueller/wireshark-corpus-mrs
**Pinned corpus commit:** ddcaa22b51c68f594e425a23388c3a2086813054
**Notebook base commit:** 5b3976c6242fddf4bb492e2b41ac17666d9c1abe
**Direction:** highest-numbered available previously unreviewed toward older.
**Exact reviewed membership (50 distinct MRs, descending):** !8761, !8760, !8759, !8758, !8757, !8756, !8755, !8754, !8753, !8752, !8751, !8750, !8749, !8748, !8747, !8746, !8745, !8744, !8743, !8742, !8741, !8740, !8739, !8738, !8737, !8736, !8735, !8734, !8733, !8732, !8731, !8730, !8729, !8728, !8727, !8726, !8725, !8724, !8723, !8722, !8721, !8720, !8719, !8718, !8717, !8716, !8715, !8714, !8713, !8712.
**Outcome tally:** 46 merged, four closed without merging (!8740, !8720, !8719, !8715).

## Review tracking and duplicate prevention

The notebook contained 371 tracking entries under reviewed-mrs-automation/ before this run. The directory inventory identified the lowest ordinary ledger as !8762–!8811, with no lower-numbered completed-range file. Exceptional gap, noncontiguous, backfill and exact-list tracking was checked, together with reviewed-mrs.md and reviewed-mrs-automation/reviewed-mrs-automation.md; none listed a completed review below !8762. The preceding ledger mentions !8761 only as an expected *future* candidate. The user-requested membership check does not infer completion of missing numbers from a filename range; all 50 selected corpus JSON files were present at the pinned commit and not identified as reviewed in the tracking.

Historical !17571–!17620 preservation was independently verified: 50 distinct completed table rows, original Git blob SHA 9393377c3adeb3a38d502532839765e3abe771fe. That file is untouched by this commit.

## Durable findings, evidence strength and caveats

**Custom-column filtering and typed field values:** Merged !8760 supersedes !8725's comma splitting, constructing occurrence-specific display filters from actual field_info values rather than display strings. The combined expression currently uses OR; source comments acknowledge that AND might be desirable for other use cases. Merged !8724 (with merged rollback !8731 and John Thacker's objection to closed !8719) makes IPP charstrings actual filterable values instead of formatting-only placeholders. Merged !8721 adds typed filterable AMQP field-table members.

**New protocol onboarding:** Merged !8746 supplies extensive substantive review by **Alexis La Goutte** and **Jaap Keuter**: proper FT_BOOLEAN parent width, exported prototypes, reserved-field registration, true/false string reuse, consistent file/header naming, release-note entry, two illustrative captures, QA scripts and fuzzing. The contributor reported a 24-hour (86,400-second) fuzz run with about 220,123 passes on the two captures. The corpus's packet-nvme-mi.c change entry has no retained diff body, limiting independent review of that file.

**Qt and conversation APIs:** Merged !8712/!8729 map a traffic-tree proxy index back to its source before context-menu operations; !8739 fixes another base/proxy-model mismatch in profile export. Merged !8734/!8735 attempted a QObject lifetime repair, but **Jim Young's post-merge macOS crash reports** make the claim of full resolution untenable. **Simon Holesch** corrected closed !8740's wrong D-Bus conversation API usage; merged !8741 is authoritative. Merged MCTP !8746 synchronizes address, port, conversation and Exported PDU identities.

**Build and parsing boundaries:** In !8748, **Christopher Maynard** distinguished custom dissector source/header/support CMake hooks from root CMake TShark tap integration; merged !8753 documents the existing root tap variable. Merged PER !8723/!8722 fixes rounding of an exclusive bit ending position to a byte-highlight span; merged !8752 validates WLAN tap addresses and ignores uninitialized AT_NONE identities.

Merged final code is weighted more heavily than withdrawn, closed or superseded proposals. Closed !8740, !8720, !8719 and !8715 are cautionary evidence, not accepted implementations. Significant maintainer comments receive greater weight than routine approval or machine-generated messages. No Guy Harris guidance was invented for this batch. Review depth labels distinguish selected code/discussion analysis from scans of routine metadata, small diffs or backports. No local Wireshark builds, capture replays or independent fuzz runs were performed.

## Exact MR assessments

| MR | Outcome | Depth | Assessment |
| --- | --- | --- | --- |
| [!8761](https://gitlab.com/wireshark/wireshark/-/merge_requests/8761) | merged | deep | LoRaWAN Class-B/beacons, v1.0.4 MAC commands and join-accept decryption; packet/key edge cases and sample captures described. |
| [!8760](https://gitlab.com/wireshark/wireshark/-/merge_requests/8760) | merged | deep | Custom-column filters derive per-field typed expressions with correct escaping; OR semantics for multiple occurrences remain a policy choice. |
| [!8759](https://gitlab.com/wireshark/wireshark/-/merge_requests/8759) | merged | scanned | Stable GSM Osmux backport: AMR_SID payload is 39 bits, rounded up to five octets; same change as !8757. |
| [!8758](https://gitlab.com/wireshark/wireshark/-/merge_requests/8758) | merged | scanned | Second stable backport of !8757 AMR_SID payload-length correction. |
| [!8757](https://gitlab.com/wireshark/wireshark/-/merge_requests/8757) | merged | discussion | GSM Osmux AMR_SID frame type consumes ceil(39/8)=5 octets, not four. |
| [!8756](https://gitlab.com/wireshark/wireshark/-/merge_requests/8756) | merged | deep | RTP supplies packet/layer-scoped padding metadata to EVS for compact versus header-full detection. |
| [!8755](https://gitlab.com/wireshark/wireshark/-/merge_requests/8755) | merged | scanned | GOOSE compatibility backport: restore hidden filterable floating_point IEEE 754 field. |
| [!8754](https://gitlab.com/wireshark/wireshark/-/merge_requests/8754) | merged | scanned | Second stable GOOSE compatibility backport of !8750. |
| [!8753](https://gitlab.com/wireshark/wireshark/-/merge_requests/8753) | merged | discussion | Document root CMake CUSTOM_TSHARK_TAP_SRC, prompted by custom-dissector build review. |
| [!8752](https://gitlab.com/wireshark/wireshark/-/merge_requests/8752) | merged | deep | Initialize WLAN header addresses and reject AT_NONE identities in conversation/endpoint taps to prevent random entries. |
| [!8751](https://gitlab.com/wireshark/wireshark/-/merge_requests/8751) | merged | scanned | Move last-used-directory Qt helper out of profile dialog into reusable UI utilities. |
| [!8750](https://gitlab.com/wireshark/wireshark/-/merge_requests/8750) | merged | discussion | Restore removed GOOSE floating_point filter as a hidden compatibility field; stable !8754/!8755 repeat it. |
| [!8749](https://gitlab.com/wireshark/wireshark/-/merge_requests/8749) | merged | scanned | BGP RFC 8365 Ethernet VPN route support; no substantive review discussion retained. |
| [!8748](https://gitlab.com/wireshark/wireshark/-/merge_requests/8748) | merged | deep | CMake custom dissector header/support source hooks; Christopher Maynard separates tap integration and requests linked issue. |
| [!8747](https://gitlab.com/wireshark/wireshark/-/merge_requests/8747) | merged | scanned | F1AP ASN.1 v17.2.0 refresh with regenerated F1AP/NR-RRC sources; large generated diff. |
| [!8746](https://gitlab.com/wireshark/wireshark/-/merge_requests/8746) | merged | deep | MCTP/control/NVMe-MI; Alexis La Goutte and Jaap Keuter require field/prototype checks, standard headers, captures, release notes and fuzzing. |
| [!8745](https://gitlab.com/wireshark/wireshark/-/merge_requests/8745) | merged | scanned | GitLab issue template documents how to upload capture attachments. |
| [!8744](https://gitlab.com/wireshark/wireshark/-/merge_requests/8744) | merged | scanned | TLS follow tap populates server address/port metadata for stream output. |
| [!8743](https://gitlab.com/wireshark/wireshark/-/merge_requests/8743) | merged | scanned | QUIC typo correction; no new durable convention. |
| [!8742](https://gitlab.com/wireshark/wireshark/-/merge_requests/8742) | merged | scanned | E2AP ASN.1 field range adjusted after ans2wrs.py correction. |
| [!8741](https://gitlab.com/wireshark/wireshark/-/merge_requests/8741) | merged | deep | Use conversation_set_conv_addr_port_endpoints for D-Bus as advised by Simon Holesch; supersedes closed !8740. |
| [!8740](https://gitlab.com/wireshark/wireshark/-/merge_requests/8740) | closed | discussion | Incorrect D-Bus conversation_new replacement, rejected for API-semantic mismatch and rewritten in merged !8741. |
| [!8739](https://gitlab.com/wireshark/wireshark/-/merge_requests/8739) | merged | discussion | Single-profile export selects base-model index rather than incompatible proxy index; associated invalid coloring rules. |
| [!8738](https://gitlab.com/wireshark/wireshark/-/merge_requests/8738) | merged | discussion | 802.11 typed protocol-tree item checker warnings now covered by CI; upstream reviewer informed. |
| [!8737](https://gitlab.com/wireshark/wireshark/-/merge_requests/8737) | merged | scanned | Remove FTP offset dead store identified by Clang Analyzer. |
| [!8736](https://gitlab.com/wireshark/wireshark/-/merge_requests/8736) | merged | scanned | wifidump explicitly enables monitor-mode control and other-BSS flags for drivers requiring them. |
| [!8735](https://gitlab.com/wireshark/wireshark/-/merge_requests/8735) | merged | scanned | Stable backport of status-bar profile-dialog QObject lifetime change from !8734. |
| [!8734](https://gitlab.com/wireshark/wireshark/-/merge_requests/8734) | merged | deep | Parented heap dialog plus WA_DeleteOnClose; Jim Young reports persistent macOS crashes after merge. |
| [!8733](https://gitlab.com/wireshark/wireshark/-/merge_requests/8733) | merged | scanned | Follow-up PVFS2 parsing adjustment to an earlier change. |
| [!8732](https://gitlab.com/wireshark/wireshark/-/merge_requests/8732) | merged | scanned | PVFS2 validates raw strings as ASCII instead of displaying unchecked bytes. |
| [!8731](https://gitlab.com/wireshark/wireshark/-/merge_requests/8731) | merged | discussion | Revert wrong IPP NULL-value change before corrected typed-field implementation in !8724. |
| [!8730](https://gitlab.com/wireshark/wireshark/-/merge_requests/8730) | merged | scanned | Backport removes inappropriate byte unit suffix from ATAP packet-count view. |
| [!8729](https://gitlab.com/wireshark/wireshark/-/merge_requests/8729) | merged | scanned | Backport of TrafficTree QModelIndex mapToSource correction !8712. |
| [!8728](https://gitlab.com/wireshark/wireshark/-/merge_requests/8728) | merged | scanned | Scheduled manufacturer/services/translations metadata regeneration. |
| [!8727](https://gitlab.com/wireshark/wireshark/-/merge_requests/8727) | merged | scanned | Scheduled release-branch generated metadata and documentation update. |
| [!8726](https://gitlab.com/wireshark/wireshark/-/merge_requests/8726) | merged | scanned | Scheduled automated vendor and documentation tables update. |
| [!8725](https://gitlab.com/wireshark/wireshark/-/merge_requests/8725) | merged | discussion | Early multi-occurrence custom-column membership filter uses comma splitting; superseded by field-aware !8760. |
| [!8724](https://gitlab.com/wireshark/wireshark/-/merge_requests/8724) | merged | deep | IPP stores actual ASCII string in filterable field and prepends explanation; John Thacker requested clean revert history. |
| [!8723](https://gitlab.com/wireshark/wireshark/-/merge_requests/8723) | merged | deep | PER restricted-string item range uses ceil(end_bit/8) to avoid off-by-one byte highlights. |
| [!8722](https://gitlab.com/wireshark/wireshark/-/merge_requests/8722) | merged | scanned | PER restricted-string range backport, same correction as !8723. |
| [!8721](https://gitlab.com/wireshark/wireshark/-/merge_requests/8721) | merged | deep | AMQP field tables/arrays gain typed name, type and value fields for filters and JSON rather than labels only. |
| [!8720](https://gitlab.com/wireshark/wireshark/-/merge_requests/8720) | closed | discussion | Unmerged IPP NULL-formatting cherry-pick; incorrect family superseded by !8724. |
| [!8719](https://gitlab.com/wireshark/wireshark/-/merge_requests/8719) | closed | discussion | John Thacker identifies wrong stored IPP field value in attempted NULL fix; not accepted. |
| [!8718](https://gitlab.com/wireshark/wireshark/-/merge_requests/8718) | merged | scanned | EVS AMR-WB 24.4 kbps enum/spec-label repair backport. |
| [!8717](https://gitlab.com/wireshark/wireshark/-/merge_requests/8717) | merged | scanned | Second stable EVS enum-label correction matching 3GPP TS 26.445. |
| [!8716](https://gitlab.com/wireshark/wireshark/-/merge_requests/8716) | merged | discussion | CFDP exposes explicit filename/message length fields and advances variable-length offsets consistently. |
| [!8715](https://gitlab.com/wireshark/wireshark/-/merge_requests/8715) | closed | discussion | Superseded profile-export fix containing unrelated IPP change; merged focused !8739 is accepted. |
| [!8714](https://gitlab.com/wireshark/wireshark/-/merge_requests/8714) | merged | scanned | TrafficTree CSV/YAML exports include previously missing column headers. |
| [!8713](https://gitlab.com/wireshark/wireshark/-/merge_requests/8713) | merged | scanned | ATAP packet-count view removes erroneous bytes suffix; !8730 backport. |
| [!8712](https://gitlab.com/wireshark/wireshark/-/merge_requests/8712) | merged | deep | Map TrafficTree proxy QModelIndex back to source before context-menu actions, avoiding wrong conversation selection. |

## Next candidate

!8711 exists in the pinned corpus and is the next expected candidate, subject to the next run's complete membership check. The recurring automation has not been paused or disabled.
