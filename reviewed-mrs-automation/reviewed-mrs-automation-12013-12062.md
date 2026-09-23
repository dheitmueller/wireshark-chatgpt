# Wireshark MR review automation ledger — !12013–!12062

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Before selecting this batch, the available review tracking on notebook `main` was reconciled, including `reviewed-mrs.md`, the supplemental automation tracker, and the per-run ledgers under `reviewed-mrs-automation/`. The immediately preceding exact ledger explicitly marked !12062 only as a frontier probe, not as reviewed. The historical !17571–!17620 batch remains preserved and counted. Numeric interval coverage was not assumed; candidate MR numbers were checked against the available tracking before this batch was selected.

Exact reviewed set (50 MRs): !12062, !12061, !12060, !12059, !12058, !12057, !12056, !12055, !12054, !12053, !12052, !12051, !12050, !12049, !12048, !12047, !12046, !12045, !12044, !12043, !12042, !12041, !12040, !12039, !12038, !12037, !12036, !12035, !12034, !12033, !12032, !12031, !12030, !12029, !12028, !12027, !12026, !12025, !12024, !12023, !12022, !12021, !12020, !12019, !12018, !12017, !12016, !12015, !12014, !12013

Outcome: 45 merged, 4 closed/unmerged (!12051, !12045, !12029, !12013), and 1 open/unmerged corpus snapshot (!12016). Closed/open work is retained as lower-weight discussion evidence and is not treated as accepted implementation precedent.

| MR | Outcome | Review depth | Durable evidence / result |
|---|---|---|---|
| !12062 | Merged | Deep / corroboration | Guy Harris lets `ws_strtou32()` validate the whole TShark frame-number argument instead of maintaining a duplicate end-pointer check; reinforces using the shared parser's complete validation contract. |
| !12061 | Merged | Deep / promoted | Guy Harris changes `-t` timestamp precision handling from a whitelist of selected resolutions to numeric parsing over the full supported 0–9 domain. Promoted with the related timestamp series to `timestamp-formatting-conventions.md`. |
| !12060 | Merged | Deep / promoted | Guy Harris removes remaining selected timestamp-resolution cases, uses domain-sized tables/calculation and `WS_TSPREC_MAX` rather than literal 9. Promoted to `timestamp-formatting-conventions.md`. |
| !12059 | Merged | Deep / promoted | John Thacker fixes SCCP RSN double shifting/masking by passing encoded bytes through `proto_tree_add_item()` rather than manually shifting and then applying the registered field mask again. Promoted to `field-decoding-api-conventions.md`. |
| !12058 | Merged | Scanned | Adds the missing `cmake-rpm-macros` dependency to MinGW RPM setup; build-environment maintenance. |
| !12057 | Merged | Scanned | Normalizes DLT dissector protocol-name definitions with the common PNAME/PSNAME/PFNAME conventions; consistency/greppability cleanup. |
| !12056 | Merged | Scanned | Release-note maintenance. |
| !12055 | Merged | Scanned | PCEP cleanup removes an unnecessary include and fixes a display-filter name. |
| !12054 | Merged | Deep / promoted | Guy Harris migrates timestamp conversion paths to the shared fractional-seconds formatter, reducing duplicated precision logic. Promoted as part of `timestamp-formatting-conventions.md`. |
| !12053 | Merged | Backport evidence | Release-4.0 documentation update for conversation types; companion/backport evidence for master !12038. |
| !12052 | Merged | Deep / submission corroboration | ZigBee Direct new dissector review required checker cleanup, sample capture, release-note entry, correct generated-field handling, and resolution of duplicate/invalid field registrations. Alexis La Goutte and Martin Mathieson supplied substantive review. Reinforces existing new-dissector/checker conventions. |
| !12051 | Closed / unmerged | Down-weighted | Aruba RADIUS dictionary update; review noted commit-message cleanup and pipeline failed. Not accepted implementation precedent. |
| !12050 | Merged | Scanned | Adds personal manufacturer name-resolution configuration; accepted narrower portion of the broader name-resolution experimentation seen in closed !12013. |
| !12049 | Merged | Deep / corroboration | John Thacker frees field values when a protocol-tree node is not added, reinforcing ownership cleanup on failed insertion paths. |
| !12048 | Merged | Scanned | `wslog` internal cleanup of `log_write_do_work()`; localized refactor. |
| !12047 | Merged | Scanned | SMB2 FSCTL_GET_NTFS_VOLUME_DATA support; protocol feature work. |
| !12046 | Merged | Deep / corroboration | John Thacker fixes `proto_tree_add_bytes_item` leaks and delays `GByteArray` allocation until length validation for exception-capable paths. Reinforces exception-safe ownership/lazy allocation guidance already present in the notebook. |
| !12045 | Closed / unmerged | Down-weighted | Empty draft INSTALL update (`changes_count` 0); maintainer noted the change was missing. No implementation precedent. |
| !12044 | Merged | Deep / promoted / high authority | Guy Harris fixes multi-step fixed-buffer timestamp formatting by tracking current pointer and remaining capacity, returns bytes formatted, and validates nanosecond normalization. Promoted to `path-and-buffer-conventions.md`. |
| !12043 | Merged | Scanned | PCEP SRv6 extension support with sample capture; protocol-specific feature. |
| !12042 | Merged | Scanned | Documentation typo cleanup. |
| !12041 | Merged | Deep / promoted | Guy Harris introduces shared `format_fractional_part_nsecs()` to centralize fractional timestamp formatting. Promoted as part of `timestamp-formatting-conventions.md`. |
| !12040 | Merged | Deep / corroboration | `addr_resolv` resets freed global path state to NULL, preventing later use-after-free. Lifetime/state cleanup; existing ownership conventions cover it. |
| !12039 | Merged | Deep / boundary corroboration | MessagePack dispatcher is corrected to cover all integer formats and exact fixint boundaries already supported by its decoder. Reinforces testing dispatch-domain completeness and boundary values. |
| !12038 | Merged | Scanned | Master documentation update describing conversation types; source of the !12053 backport. |
| !12037 | Merged | Scanned | S12AP/X2AP/NGAP EIA7 capability-bit dissection; protocol feature. |
| !12036 | Merged | Deep / compatibility evidence | Martin Mathieson restores natural Lua custom packet callback field order and the accidentally omitted `frame` field after checking intent with the original author. Useful API-output compatibility evidence; no separate rule promoted. |
| !12035 | Merged | Scanned | IEEE 802.11 ML/TWT field additions; protocol feature. |
| !12034 | Merged | Scanned | RDP enhancements and cleanup including Azure Cookie/NegoToken handling; protocol-specific. |
| !12033 | Merged | Deep / promoted | Guy Harris adds common nstime-to-ISO8601 formatting and replaces duplicated call-site logic, supporting the centralized timestamp-formatting convention. |
| !12032 | Merged | Tooling corroboration | Fixes PCEP field masks reported by `tools/check_typed_item_calls.py`; reinforces treating checker diagnostics as correctness signals. |
| !12031 | Merged | Deep / promoted | Guy Harris updates epoch-time formatting to consume `nstime_t` and support the full timestamp precision range. Included in `timestamp-formatting-conventions.md`. |
| !12030 | Merged | Deep / promoted | Guy Harris updates signed-time formatting to consume `nstime_t` and support all precisions from seconds through nanoseconds. Included in `timestamp-formatting-conventions.md`. |
| !12029 | Closed / unmerged draft | Discussion-focused / down-weighted | Gerald Combs investigated enabling stricter macOS availability warnings, then documented that the default warning already covers APIs newer than the project's deployment targets. Closed without merge; retained only as evidence to validate compiler-warning policy against actual targets. |
| !12028 | Merged | Scanned | GitLab CI stops forcing `CMAKE_OSX_SYSROOT`; platform/build configuration maintenance. |
| !12027 | Merged | Scanned | Adds minimum macOS version information for Qt 6.5; build compatibility metadata. |
| !12026 | Merged | Scanned | About dialog reports Wayland/Xorg GUI environment; localized UI diagnostics. |
| !12025 | Merged | Scanned | Automated master data update. |
| !12024 | Merged | Scanned | Automated release-4.0 data update. |
| !12023 | Merged | Scanned | Automated release-3.6 data update. |
| !12022 | Merged | Scanned | Stable-branch Qt overlay scrollbar styling fix for dark mode; UI/platform maintenance. |
| !12021 | Merged | Scanned | X.509 certificate-extension support; protocol feature. |
| !12020 | Merged | Deep / promoted / high authority | Guy Harris changes the capture child to send the structured `errno` from dumpcap `exec` failure instead of only user-facing text, allowing the parent to distinguish permission failure without parsing prose. Promoted to `capture-diagnostic-conventions.md`. |
| !12019 | Merged | Scanned | Windows CI/test configuration disables GUI where appropriate and skips GUI tests in that job. |
| !12018 | Merged | Scanned | `wslog` timestamp-code cleanup; localized common-code maintenance. |
| !12017 | Merged | Deep / promoted / high authority | Guy Harris changes ISO8601/Unix-time parser return values from an 8-bit count to an end pointer with NULL on error, forcing callers to handle malformed required timestamps explicitly. Promoted to `c-api-call-contract-conventions.md`. |
| !12016 | Open / unmerged snapshot | Discussion-focused / down-weighted | Proposed X.29 Follow Stream support. Alexis requested a capture and the contributor supplied one, but the corpus snapshot remains open with unresolved discussions; useful submission evidence only. |
| !12015 | Merged | Deep / submission corroboration | New Z21 dissector review required release-note integration, analyzer/checkHF cleanup, duplicate value-string resolution, sample-driven checking, and discussion of Info-column style. Martin Mathieson also pointed to `check_typed_item_calls.py --extra-value-string-checks`. Reinforces existing dissector-submission/tooling conventions. |
| !12014 | Merged | Scanned | Adds the ST 2110-20 dissector with specification and sample-capture references; new protocol support. |
| !12013 | Closed / unmerged | Down-weighted | Broad name-resolution rewrite was explicitly described by its author as completely untested/probably buggy and was later closed. Not accepted precedent; narrower !12050 was merged separately. |

## Notebook updates promoted in this run

- `path-and-buffer-conventions.md`: !12044 establishes cursor-plus-remaining-capacity state for multi-step fixed-buffer formatting, with a helper return contract expressed in bytes.
- `capture-diagnostic-conventions.md`: Guy Harris's !12020 establishes structured error/status transport across helper-process IPC rather than control flow based on parsing user-facing prose.
- `field-decoding-api-conventions.md`: John Thacker's !12059, reinforced by previously reviewed stable backports !12063/!12064, establishes that a registered field's mask/shift must be applied exactly once.
- `c-api-call-contract-conventions.md`: Guy Harris's !12017 establishes end-pointer-plus-failure-sentinel contracts for variable-length text parsers when callers need exact parse progress and explicit malformed-input handling.
- New `timestamp-formatting-conventions.md`: Guy Harris's merged !12030/!12031/!12033/!12041/!12054/!12060/!12061 sequence establishes centralized timestamp formatting and treating precision as the complete supported numeric domain rather than a whitelist of favored resolutions.

Strong corroboration intentionally not duplicated into additional notebook rules includes exception-safe/lazy ownership in !12046/!12049, checker/new-dissector workflow evidence in !12052/!12015/!12032, and boundary/dispatch completeness in !12039. The open or closed work in !12016/!12051/!12045/!12029/!12013 was deliberately down-weighted.

Frontier check only (not reviewed): !12012, `strptime: Try to make "%s" code portable`, exists in the corpus and is merged. It is the next descending candidate absent newly scraped higher-numbered unreviewed material.
