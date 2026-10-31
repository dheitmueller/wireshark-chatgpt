# Automated Wireshark MR review — !8660 through !8611

**Review date:** 2026-10-31. **Model:** GPT-6 (newer than GPT-5.6 Sol).
**Corpus repository:** `dheitmueller/wireshark-corpus-mrs`
**Corpus commit reviewed:** `ddcaa22b51c68f594e425a23388c3a2086813054`
**Notebook baseline commit:** `37cbed32e26721abafe8842c8b439dcdcf3ca1b9`
**Direction:** descending, highest-numbered previously unreviewed MR first.
**Exact reviewed count:** **50** distinct corpus MRs: **49 merged**, **1 closed without merging** (!8641).

## Tracking and selection

The baseline notebook has 373 entries under `reviewed-mrs-automation/`, inventoried from the Git tree before selection. Tracking consulted includes `reviewed-mrs.md`, the directory's supplemental `reviewed-mrs-automation.md`, previous exact-membership !8711–!8661 ledger, historical !17571–!17620 ledger, noncontiguous/gap/backfill/exact-list records, and the ordered per-run tracking inventory. Previously accepted interval names were not used as proof of completed individual entries. The preceding run names !8660 only as an unreviewed *next candidate*, and existing completed-review tracking has no entries for the new !8660–!8611 candidate window.

The pinned corpus tree contains all 50 individual files `mr_8660.json` through `mr_8611.json` and lists !8610 as the next available record. All 50 stored JSON records' metadata, diff changes, and discussions were inspected. This is historical corpus review, not a live verification of current GitLab MR state.

The historical !17571–!17620 run is preserved and counted: its exact 50 reviewed entries remain in `reviewed-mrs-automation-17571-17620.md` with blob SHA `9393377c3adeb3a38d502532839765e3abe771fe`. No existing ledgers are edited by this run.

## Exact reviewed membership

| MR | Corpus outcome | Review depth | Finding |
| --- | --- | --- | --- |
| [!8660](https://gitlab.com/wireshark/wireshark/-/merge_requests/8660) | merged | deep | João Valverde adds @-prefixed raw field addressing as FT_BYTES, distinct VM/reference handling, captured-length clipping, docs/tests and release notes. |
| [!8659](https://gitlab.com/wireshark/wireshark/-/merge_requests/8659) | merged | scanned | Stable-branch cherry-pick of !8651: preserve the raw IPv4 fragment header value and separately format the offset in bytes. |
| [!8658](https://gitlab.com/wireshark/wireshark/-/merge_requests/8658) | merged | scanned | SCTP Host Name Address parameter backport; extracts printable hostname through the tree string-return API and marks field FT_STRINGZ. |
| [!8657](https://gitlab.com/wireshark/wireshark/-/merge_requests/8657) | merged | scanned | Supported-branch backport of dotted H.223/H.264/H.265 Protocol-column labels. |
| [!8656](https://gitlab.com/wireshark/wireshark/-/merge_requests/8656) | merged | scanned | H.248 naming fix updates both ASN.1 template and generated code, plus displayed dissector-table name. |
| [!8655](https://gitlab.com/wireshark/wireshark/-/merge_requests/8655) | merged | scanned | PER callback length-unit correction backport: open-type octets multiplied by eight for bit-count API. |
| [!8654](https://gitlab.com/wireshark/wireshark/-/merge_requests/8654) | merged | scanned | Second PER callback length-unit correction backport; corroborates !8645. |
| [!8653](https://gitlab.com/wireshark/wireshark/-/merge_requests/8653) | merged | scanned | Python PNG compressor migration fixes option handling and tool-detection loop; localized tool maintenance. |
| [!8652](https://gitlab.com/wireshark/wireshark/-/merge_requests/8652) | merged | scanned | Windows GitHub workflow updates winflexbison3 Chocolatey acquisition; stable counterpart of !8633. |
| [!8651](https://gitlab.com/wireshark/wireshark/-/merge_requests/8651) | merged | deep | IPv4 fragment offset: proto_tree_add_uint_format_value takes unmasked container value; separate displayed scale avoids double bitmask. |
| [!8650](https://gitlab.com/wireshark/wireshark/-/merge_requests/8650) | merged | deep | 802.15.4 TAP PHR addition; Alexis La Goutte requests representative capture and static-analysis cleanup; contributor supplies capture and splits distinct dissector changes. |
| [!8649](https://gitlab.com/wireshark/wireshark/-/merge_requests/8649) | merged | deep | S7Comm invalid UTF-8 mapping fixed through char_val_to_str with hexadecimal fallback; John Thacker requests exact API naming. |
| [!8648](https://gitlab.com/wireshark/wireshark/-/merge_requests/8648) | merged | scanned | TCP TARR draft revision changes rate field and option-length handling; protocol-version-specific update. |
| [!8647](https://gitlab.com/wireshark/wireshark/-/merge_requests/8647) | merged | scanned | IEEE 802.11 DSCP strbuf formatting correction in maintained branch; handles repeated append safely. |
| [!8646](https://gitlab.com/wireshark/wireshark/-/merge_requests/8646) | merged | scanned | Second IEEE 802.11 DSCP strbuf formatting backport; no separate implementation convention. |
| [!8645](https://gitlab.com/wireshark/wireshark/-/merge_requests/8645) | merged | deep | Master PER OID open type passes type_length*8 to tvb_new_octet_aligned because the API expects bits. |
| [!8644](https://gitlab.com/wireshark/wireshark/-/merge_requests/8644) | merged | scanned | IEEE 802.11 DSCP response summary switches from looped snprintf to wmem_strbuf; historical new_label use later superseded. |
| [!8643](https://gitlab.com/wireshark/wireshark/-/merge_requests/8643) | merged | deep | Gerald Combs repairs IEEE 802.11 SCIDX/angle string assembly with allocator-backed append instead of drift-prone snprintf offsets. |
| [!8642](https://gitlab.com/wireshark/wireshark/-/merge_requests/8642) | merged | discussion | Local gitlab-runner exec requires CI variables to be strings; quote numeric YAML scalars. Gerald Combs comments on hosted/local divergence. |
| [!8641](https://gitlab.com/wireshark/wireshark/-/merge_requests/8641) | closed | scanned | Earlier matching GitLab CI scalar-string proposal closed; accepted implementation is !8642, not this MR. |
| [!8640](https://gitlab.com/wireshark/wireshark/-/merge_requests/8640) | merged | scanned | H.224 field label corrects Sequence to Segment; protocol terminology hygiene. |
| [!8639](https://gitlab.com/wireshark/wireshark/-/merge_requests/8639) | merged | discussion | Thrift SET/LIST field descriptions corrected; Anders Broman questions copy/paste text and review resolves narrow naming fix. |
| [!8638](https://gitlab.com/wireshark/wireshark/-/merge_requests/8638) | merged | deep | John Thacker applies FT_STRINGZ and decoded display return to SCTP counted-and-NUL-terminated hostname; avoids UTF-8 width confusion. |
| [!8637](https://gitlab.com/wireshark/wireshark/-/merge_requests/8637) | merged | scanned | Guy Harris cleans H.224 protocol naming in comments; no substantive new implementation pattern. |
| [!8636](https://gitlab.com/wireshark/wireshark/-/merge_requests/8636) | merged | scanned | Dotted H.223/H.224/H.264/H.265 Protocol-column labels follow standard punctuation; !8657 stable counterpart. |
| [!8635](https://gitlab.com/wireshark/wireshark/-/merge_requests/8635) | merged | scanned | Metamako CRC64 hexadecimal output uses PRIx64, correcting earlier PRIu64 format; review of integer semantic display. |
| [!8634](https://gitlab.com/wireshark/wireshark/-/merge_requests/8634) | merged | deep | John Thacker uses ENC_APN_STR for GTP/GTPv2 RFC 1035-style FQDN label encoding; retains documented misencoded ASCII fallback. |
| [!8633](https://gitlab.com/wireshark/wireshark/-/merge_requests/8633) | merged | scanned | winflexbison3 GitHub-hosted Chocolatey version in Windows workflow; corresponding stable backport !8652. |
| [!8632](https://gitlab.com/wireshark/wireshark/-/merge_requests/8632) | merged | discussion | Bluetooth VCS characteristic decoding; Alexis La Goutte corrects mislabeled Flags/Mute fields and spelling. |
| [!8631](https://gitlab.com/wireshark/wireshark/-/merge_requests/8631) | merged | scanned | Clang Analyzer dead-code cleanup across Diameter/MBIM/O-RAN/PFCP; narrowly scoped without new architecture rule. |
| [!8630](https://gitlab.com/wireshark/wireshark/-/merge_requests/8630) | merged | scanned | Automated registry/certificate/manufacturer/translation updates; no independent policy conclusion. |
| [!8629](https://gitlab.com/wireshark/wireshark/-/merge_requests/8629) | merged | scanned | Automatic generated registry and manufacturer data update on another maintained branch. |
| [!8628](https://gitlab.com/wireshark/wireshark/-/merge_requests/8628) | merged | scanned | Automatic generated registry and packaging metadata; release note reflects Windows Qt bundled version. |
| [!8627](https://gitlab.com/wireshark/wireshark/-/merge_requests/8627) | merged | discussion | BLF CAN-FD RTR/SRR interpretation fix; remote flag meaning depends on record type, stable counterpart of !8626. |
| [!8626](https://gitlab.com/wireshark/wireshark/-/merge_requests/8626) | merged | scanned | Master BLF CAN-FD flag fix; do not apply classic CAN remote-frame flag to CAN FD payload. |
| [!8625](https://gitlab.com/wireshark/wireshark/-/merge_requests/8625) | merged | discussion | X2AP private IE dispatch adds PER OID callback via ASN.1 cnf and generated code; bit/octet boundary correctness matters. |
| [!8624](https://gitlab.com/wireshark/wireshark/-/merge_requests/8624) | merged | scanned | Qt Resolved Addresses dialog now displays capture filename when available; narrow UI presentation improvement. |
| [!8623](https://gitlab.com/wireshark/wireshark/-/merge_requests/8623) | merged | deep | John Thacker reverses WOW ASCII octets before charset decoding; reversing UTF-8 after substitution corrupts multibyte replacement. |
| [!8622](https://gitlab.com/wireshark/wireshark/-/merge_requests/8622) | merged | deep | Portable TRY/CATCH tvbuff test variables made volatile and initialized; guard wide-varint boundary; João Valverde explicitly approves. |
| [!8621](https://gitlab.com/wireshark/wireshark/-/merge_requests/8621) | merged | scanned | Win64 packaging CI supplies explicit Qt CMAKE_PREFIX_PATH; historical build environment fix. |
| [!8620](https://gitlab.com/wireshark/wireshark/-/merge_requests/8620) | merged | discussion | Gerald Combs removes redundant Latest Clang job covered by Code Checks + Clang Warnings; preserve distinct coverage. |
| [!8619](https://gitlab.com/wireshark/wireshark/-/merge_requests/8619) | merged | scanned | Gerald Combs schedules expensive RPM test jobs twice daily rather than on each merge. |
| [!8618](https://gitlab.com/wireshark/wireshark/-/merge_requests/8618) | merged | deep | Gerald Combs schedules Linux RPM/APT packaging twice daily due to cache/cost, adds explicit schedule rules, removes unnecessary CMake tweak. |
| [!8617](https://gitlab.com/wireshark/wireshark/-/merge_requests/8617) | merged | scanned | GitHub MSYS2 workflow YAML naming/style consistency only. |
| [!8616](https://gitlab.com/wireshark/wireshark/-/merge_requests/8616) | merged | deep | João Valverde defines packet-column/label display-escaping boundary using ws_label_strcpy/strcat and UTF-8 checks plus whitespace unit tests; later fixes refine edge cases. |
| [!8615](https://gitlab.com/wireshark/wireshark/-/merge_requests/8615) | merged | scanned | GitHub lockdown workflow YAML consistency only. |
| [!8614](https://gitlab.com/wireshark/wireshark/-/merge_requests/8614) | merged | discussion | John Thacker uses ASCII-aware tvb_get_string_enc for MPEG ISO-639 descriptor display instead of passing raw bytes. |
| [!8613](https://gitlab.com/wireshark/wireshark/-/merge_requests/8613) | merged | scanned | Couchbase unnecessary offset assignment removed after Clang Analyzer report. |
| [!8612](https://gitlab.com/wireshark/wireshark/-/merge_requests/8612) | merged | scanned | H.224 human-facing H.224 label remains dotted while canonical display-filter protocol name is h224. |
| [!8611](https://gitlab.com/wireshark/wireshark/-/merge_requests/8611) | merged | deep | Guy Harris uses the high bit to classify ASCII-range octets in packet-bytes dialog, independent of plain-char signedness. |

## Durable notebook changes and weighting

New observations were incorporated into `dfilter-conventions.md`, `text-encoding-conventions.md`, `field-mask-semantics-conventions.md`, `c-portability-conventions.md`, `ci-resource-scheduling-conventions.md`, `wire-encoding-conventions.md`, `string-api-conventions.md`, `column-string-conventions.md`, `ci-tooling-conventions.md`, and `dissector-naming-conventions.md`.

Most significant accepted evidence: João Valverde's raw-byte display-filter VM architecture (!8660), his display-string boundary refactor (!8616) and char-value helper (!8649); John Thacker's correct wire-string handling (!8634, !8638, !8623); Guy Harris's char-signedness reasoning (!8611); Gerald Combs's explicit CI resource policy (!8618–!8620) and bounded string-assembly fixes (!8643/!8644); PER bit/octet callback contract (!8645 and stable backports !8654/!8655). Alexis La Goutte's request for 802.15.4 sample captures and static analysis follow-up (!8650) is accepted reviewer guidance, not a claim that this run replayed the supplied captures.

The sole closed item, !8641, is lower-weight negative/superseded evidence; the equivalent merged !8642 is the implementation precedent. Stable-branch cherry-picks corroborate but are not counted as independent design discoveries. Critically, the old 2022 use of `wmem_strbuf_new_label()` in !8643/!8644 is superseded by later merged !8933/!8946: the notebook promotes incremental formatting, **not** the later-rejected byte-limited constructor.

**Evidence limits:** No compilation, packet replay, independent tests, or fuzzing in this documentation review. Some historic records contain only system/merge chatter; review depth is reported per row rather than inventing substantive human comments.

**Next expected candidate:** **!8610** in the pinned corpus, subject to a fresh complete tracking/membership check. Keep the existing recurring review automation enabled.
