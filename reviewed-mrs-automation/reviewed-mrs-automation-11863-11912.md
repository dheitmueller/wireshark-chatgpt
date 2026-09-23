# Wireshark MR review automation: !11863–!11912

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Selection method: reconstructed the already-reviewed set from the available notebook tracking, including `reviewed-mrs.md`, the aggregate tracking represented by prior automation runs, and the per-run files in `reviewed-mrs-automation/`. The historical !17571–!17620 batch remains preserved and counted. The preceding run's lookup of !11912 was only a frontier check, not a review. No existing tracking entry was found for this !11863–!11912 set, so the fifty highest-numbered unreviewed corpus MRs are exactly !11912 down through !11863. Numeric range coverage was not assumed merely from neighboring ledgers.

Status weighting: merged master work is treated as the strongest implementation evidence. Closed/unmerged !11900 and !11882 were reviewed but down-weighted. Backports and follow-ups were used primarily as corroboration of the accepted master behavior.

| MR | Review status | Outcome and durable review note |
|---|---|---|
| !11912 | Deep | Merged John Thacker TVBuff cleanup. Replaces obvious `tvb_new_subset_length_caplen()` uses with `tvb_new_subset_length()` / `tvb_new_subset_remaining()` so captured length is not fabricated from logical length; promoted with !11864 to `tvbuff-parsing-conventions.md`. |
| !11911 | Deep | Merged John Thacker QUIC multipath fix. Tracks maximum packet number separately per negotiated CID sequence-number space; strong corroboration that state keys must model the protocol's true sequence namespace. |
| !11910 | Scanned | Merged HTTP/3 duplicate display-filter-name correction. Correctness cleanup; no distinct new convention. |
| !11909 | Deep | Merged John Thacker HTTP/3 lifetime fix. A file-scope `wmem_map` must not retain a pointer to a stack key; copy the CID into file-scope storage before insertion. Strong corroboration of existing container key/value lifetime guidance. |
| !11908 | Deep | Merged portability series replacing project `strptime()` behavior with imported NetBSD code/wrappers. Guy Harris explicitly challenged POSIX-only `localtime_r()` assumptions and non-standard `%s`; high-authority precursor to !11917. Added to `time-parsing-conventions.md`. |
| !11907 | Scanned | Merged manufacturer-output formatting cleanup. No durable new rule. |
| !11906 | Scanned | Merged Qt Edit Resolved Name integration with PacketList/packet details. UI behavior enhancement; no distinct reusable convention extracted. |
| !11905 | Scanned | Merged documentation update for renamed/replaced Resolve Name action. Keeps docs aligned with current UI. |
| !11904 | Scanned | Merged stable-branch cherry-pick accepting RTCMEM v1/v2 as the same currently supported layout. Backport evidence only. |
| !11903 | Scanned | Merged stable-branch cherry-pick of RTCMEM v1/v2 handling. Backport evidence only. |
| !11902 | Scanned | Merged Wi-Fi NAN display-filter namespace rename to `wifi_nan`, reserving `nan` for language numeric syntax. Reinforces avoiding collisions with filter-language keywords. |
| !11901 | Deep | Merged João Valverde display-filter typing fix. Boolean literals now remain boolean values instead of being routed through value-string machinery and becoming integer-typed; also updates diagnostics, case handling, reserved words, and docs. Strong parser semantic-typing exemplar. |
| !11900 | Discussion-focused (closed/unmerged) | Aruba RADIUS dictionary update. Alexis La Goutte reviewed provenance against Aruba/FreeRADIUS data and Wireshark-specific vendor naming. Useful lower-weight dictionary/provenance discussion, but not accepted implementation precedent. |
| !11899 | Deep | Merged Guy Harris Wiretap cleanup removing unused duplicate extension metadata for magic-number readers. High-authority single-owner/dispatch-registry lesson; promoted to `registry-metadata-conventions.md`. |
| !11898 | Scanned | Merged removal of disabled/unmaintained log3gpp stubs. Reinforces deleting stale nonfunctional scaffolding rather than carrying code that drifts with APIs. |
| !11897 | Scanned | Merged Guy Harris comment expansion/correction around Wiretap metadata. Corroborates !11899 registry-ownership context. |
| !11896 | Scanned | Merged Wiretap extension-list documentation/organization cleanup. Corroborates !11899 registry-ownership context. |
| !11895 | Scanned | Merged O-RAN FH CUS display improvement showing preference-derived values when no `udCompHdr` is present. No general convention added. |
| !11894 | Scanned | Merged master RTCMEM v1/v2 compatibility change, with !11903/!11904 as backports. Accepted implementation evidence but protocol-specific. |
| !11893 | Scanned | Merged HTTP/3 Clang analyzer warning fixes. Static-analysis-driven cleanup; no new rule beyond existing analyzer guidance. |
| !11892 | Scanned | Merged WSDG formatting/admonition correction. Documentation-only. |
| !11891 | Scanned | Merged M3UA preference hint pointing users to MTP3 for shared SS7-standard behavior. UI/documentation discoverability improvement. |
| !11890 | Scanned | Merged MTP3 address-column filter-string support for SS7 point codes. Enables Apply-as-Filter semantics; no distinct new convention. |
| !11889 | Scanned | Merged DOCSIS field-mask correction caught by field metadata checking. Reinforces checker-driven field-registration correctness. |
| !11888 | Scanned | Merged manufacturer iterator optimization reducing copies per iteration. No distinct architectural lesson. |
| !11887 | Scanned | Merged HTTP/3 `val_to_str_const()` correction identified by `tools/check_val_to_str.py`. Reinforces using the checker-recommended API when the unknown text has no format conversion. |
| !11886 | Scanned | Merged WSDG section-title cleanup. Documentation-only. |
| !11885 | Scanned | Merged DOCSIS 4.0 registration TLV support. Protocol feature work without substantive reusable review guidance. |
| !11884 | Deep | Merged manufacturer lookup API optimization returning borrowed const names instead of populating an unused result structure. Valid lifetime/performance refactor; corroborates existing borrowed-pointer ownership rules. |
| !11883 | Scanned | Merged HTTP/3 internal-linkage cleanup making a function static. Scope hygiene only. |
| !11882 | Discussion-focused (closed/unmerged) | Small Aruba dictionary attribute addition. Reviewed but down-weighted because it closed unmerged and was overtaken by broader dictionary work. |
| !11881 | Scanned | Merged Guy Harris documentation expanding the meaning of extensions for Wiretap open routines. Corroborates !11899. |
| !11880 | Scanned | Merged Guy Harris documentation explaining Wiretap extension/open-info tables. Corroborates !11899's concern that multiple registries have distinct semantics and need clear ownership. |
| !11879 | Scanned | Merged Wiretap destination-address typo correction. Straight correctness fix. |
| !11878 | Scanned | Merged stable-branch ESL Ethernet nanosecond timestamp support. Backport evidence. |
| !11877 | Scanned | Merged stable-branch ESL Ethernet nanosecond timestamp support. Backport evidence. |
| !11876 | Scanned | Merged Keysight NetFlow field updates. Protocol/vendor data maintenance. |
| !11875 | Scanned | Merged DOCSIS 4.0 MDD Diplexer Band Edge Override TLV support. Protocol feature work. |
| !11874 | Scanned | Merged Wiretap `.scap` extension registration for pcapng. Registry data update; no new rule beyond the !11899 ownership lesson. |
| !11873 | Scanned | Merged SCCP DT2 Data field addition. Protocol field completeness fix. |
| !11872 | Scanned | Merged Windows nghttp2 package update to 1.55.1. Dependency maintenance. |
| !11871 | Scanned | Merged O-RAN FH CUS PRB-number display correction using the `rb` field. Protocol-specific correctness fix. |
| !11870 | Scanned | Merged `value_string` error cleanup. Reinforces existing value-string/field-semantic checking guidance. |
| !11869 | Deep/corroboration | Merged QUIC multipath decryption support, explicitly noting that separate packet-number spaces were not yet handled. The immediate merged !11911 follow-up completes that state model, so !11911 is the stronger final precedent. |
| !11868 | Scanned | Merged WSDG note about additional Visual Studio components for Arm64. Build documentation. |
| !11867 | Deep/corroboration | Merged FlexRay cleanup uses typed `proto_tree_add_item_ret_*()` extraction to avoid redundant raw reads and improves malformed-data diagnostics. Reinforces existing typed-item/parser-value guidance. |
| !11866 | Scanned | Merged Debian symbol-list update. Packaging maintenance. |
| !11865 | Deep | Merged TShark `--print-timers` instrumentation using monotonic timing and structured JSON output for passes/dissection/filtering. Useful benchmarking facility, but no additional review-derived coding convention. |
| !11864 | Deep | Merged John Thacker core TVBuff invariant: subset captured length cannot exceed reported length; API docs recommend ordinary length/remaining helpers for most callers. Promoted with !11912 to `tvbuff-parsing-conventions.md`. |
| !11863 | Discussion-focused | Merged PTP L1Sync dissection. Alexis La Goutte caught a source typo and discussed presentation of optional 2/3-byte flag fields; useful protocol-field review evidence, but no broader rule promoted from the final implementation. |

## Notebook updates from this batch

- Extended `tvbuff-parsing-conventions.md` with the captured-length ≤ reported-length subset invariant and the preference for `tvb_new_subset_length()` / `tvb_new_subset_remaining()` from merged !11864 and !11912.
- Extended `time-parsing-conventions.md` with !11908's high-authority Guy Harris portability review: avoid undocumented `strptime("%s")`, avoid unnecessary `struct tm` for numeric epoch input, and audit imported libc dependencies against all supported platforms.
- Added `registry-metadata-conventions.md` from Guy Harris's merged !11899 and surrounding Wiretap documentation cleanups: avoid duplicated unused metadata, make registry ownership explicit, and treat weak-reader precedence as part of the dispatch contract.
- Retained !11909, !11911, !11901, !11867, !11884, and other useful items as corroboration where the notebook already contains the applicable lifetime/state/typed-value rule rather than creating duplicate guidance.

Exactly 50 MRs were reviewed in this run.