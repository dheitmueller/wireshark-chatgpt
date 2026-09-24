# Automated MR review ledger: !11513–!11562

Reviewed on 2026-09-24 using GPT-5.6 Sol.

Corpus: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Notebook starting commit: `b219c71f0550e5ba67d8a7e39dc5206e9231b0cb`

Review direction: descending from the newest previously unreviewed MR toward older MRs.

Before selecting this batch, the already-reviewed set was reconstructed from the available `reviewed-mrs-automation/` per-run ledgers and `reviewed-mrs.md`. The historical !17571–!17620 batch remains explicitly preserved and counted. Selection was based on individual MR membership, not inferred numeric-range coverage. The prior mention of !11562 was only a frontier probe and therefore did not count as a review.

Exactly 50 previously unreviewed MRs were selected and reviewed in descending order:

!11562, !11561, !11560, !11559, !11558, !11557, !11556, !11555, !11554, !11553, !11552, !11551, !11550, !11549, !11548, !11547, !11546, !11545, !11544, !11543, !11542, !11541, !11540, !11539, !11538, !11537, !11536, !11535, !11534, !11533, !11532, !11531, !11530, !11529, !11528, !11527, !11526, !11525, !11524, !11523, !11522, !11521, !11520, !11519, !11518, !11517, !11516, !11515, !11514, !11513.

Merged MRs were weighted above abandoned/superseded work. !11549 and !11543 were closed without merging and were deliberately down-weighted. Stable-branch backports were treated mainly as corroboration of their corresponding accepted master changes.

## Per-MR review

| MR | Review outcome |
| --- | --- |
| !11562 | Merged. Debian packaging update for Lintian's pointed-hint override format. Straightforward packaging compatibility maintenance. |
| !11561 | Merged, authored/merged by John Thacker. Adds newly exported enterprise/services/manuf symbols to Debian ABI metadata after the associated API work. Packaging/ABI bookkeeping rather than a new architectural rule. |
| !11560 | Merged. Debian Lintian override for embedded manufacturer data; packaging-policy maintenance around generated OUI/manuf data. |
| !11559 | Merged stable-branch backport correcting the `wlan.wfa.ie.wpau.cs.oui` field abbreviation typo. Corroborates keeping registered field names synchronized across supported branches. |
| !11558 | Merged stable-branch backport of the same WFA field-abbreviation correction. |
| !11557 | Merged, authored/merged by João Valverde. Continues manufacturer-table API cleanup: `ws_manuf` namespacing, public interface cleanup, and count/iteration support. Useful API-cohesion evidence but no new rule beyond existing naming/ownership guidance. |
| !11556 | Merged master/stable field-abbreviation typo correction for WFA/WPAU. |
| !11555 | Merged, authored/merged by Gerald Combs. Restores a missing GitLab CI build step required by the manufacturer-data build path. Strong CI-pipeline correctness evidence, but narrowly scoped. |
| !11554 | Merged, authored/merged by Gerald Combs. Restricts manually started `web` pipelines to the intended branch/repository conditions, matching the policy applied to pushes. CI policy maintenance. |
| !11553 | Merged after extended rebasing/review. Adds TCP flow/turn count to conversation statistics. Accepted feature work; no new cross-cutting convention extracted. |
| !11552 | Merged. Updates RTPS security-algorithm PIDs from provisional values to their final assigned values. Standards/registry synchronization. |
| !11551 | Merged stable backport of the Qt data-size display change to SI quantifiers with zero precision. Presentation maintenance. |
| !11550 | **Deep, merged.** Adds BGP-LS SRv6 extensions with a representative capture. Alexis La Goutte caught one display-filter abbreviation registered with incompatible `FT_SYSTEM_ID` and `FT_IPv4` types; the accepted revision assigns distinct field names. Strong corroboration that incompatible field types must not share one display-filter abbreviation. |
| !11549 | **Closed/unmerged; down-weighted.** Proposed changing the TLS extended-master-secret combined mask from bitwise OR to AND. John Thacker explained that the two states occupy distinct flag bits, so OR is required to form the mask and `(state & mask) == mask` correctly tests that both are set; AND would produce zero. Also received commit-message guidance. Negative review evidence only. |
| !11548 | Merged. CI/build plumbing for manufacturer data. Corroborates ensuring generated-data paths are exercised by the build pipeline. |
| !11547 | Merged. Documents `tshark -G` dumps for manufacturer, service, and enterprise-number data. Documentation synchronized with newly exposed functionality. |
| !11546 | **Deep, merged master; authored and merged by Guy Harris.** Fixes double-free-prone Wiretap dump-parameter cleanup. `wtap_dump_params_cleanup()` already owns/frees section-header blocks, while IDB info has a distinct destructor. The accepted text2pcap helper centralizes the correct composite teardown sequence. Promoted to `cleanup-ownership-conventions.md`. |
| !11545 | Merged, authored by Jaap Keuter and approved/merged by Gerald Combs. Handles CP2179 timetag-information responses that contain no records. Good malformed/edge-case protocol handling evidence. |
| !11544 | Merged, authored/merged by João Valverde. Manufacturer lookup/iteration cleanup and broadcast/multicast-bit handling; useful API refactoring evidence but no separate durable rule. |
| !11543 | **Closed/unmerged; down-weighted.** Proposed `tshark --fields-xpath`; the author closed it as belonging to another MR and the pipeline failed. Superseded/duplicate submission, not convention evidence. |
| !11542 | Merged. Adds `tshark -G services` / `-G enterprises` dumping. Straightforward CLI/data-export feature. |
| !11541 | Merged. Updates QUIC v2 constants to final values. Standards synchronization. |
| !11540 | Merged. Adds the GUI OUI lookup tool using manufacturer data. Feature integration; no new general rule. |
| !11539 | Merged. Debian symbol-file maintenance. |
| !11538 | Merged stable-branch backport restoring WebSocket text payload exposure. Corroborating branch maintenance. |
| !11537 | **Deep/corroboration, merged.** Corrects 3GPP JSON SupportFeatures decoding where the source value is a hex-ASCII string that must be converted before bitmask interpretation. John Thacker recommended replacing fixed scratch storage with `tvb_memdup(pinfo->pool, ..., len)`, giving dynamically sized packet-lifetime storage; accepted. Reinforces packet-scope allocation and decode-before-interpretation. |
| !11536 | Merged master Qt/UI change using SI size quantifiers and zero precision. Presentation policy only. |
| !11535 | **Deep/corroboration, merged.** Fixes IEEE 802.11 typed-item width/mask mismatches reported by the checker. Martin Mathieson raised the important nuance that a typed-value API can legitimately add a narrow logical value from wider backing bytes, so checker findings must be interpreted against API semantics and representative captures. The accepted fields/masks were corrected for this case. Reinforces existing typed-item-checker guidance. |
| !11534 | Merged. CQL dead-store fix found by Clang Analyzer. Static-analysis cleanup. |
| !11533 | Merged. IEEE 802.11be draft fixes for MLO association response and EHT NDP Announcement. Protocol-specific correctness. |
| !11532 | Merged master change restoring the WebSocket text payload field. Protocol presentation compatibility. |
| !11531 | Merged release-3.6 backport of the GSM A DTAP Signalling Access Protocol value-string correction. |
| !11530 | Merged release-4.0 backport of the GSM A DTAP Signalling Access Protocol value-string correction. |
| !11529 | **Deep, merged master; authored and merged by John Thacker.** HTTP used `get_tcp_conversation_data()` only to determine direction, but HTTP can be invoked over UDP/SSDP, SCTP, or proxy paths; the TCP helper created spurious TCP conversation/stream state. The accepted code derives direction directly from packet endpoints. Promoted to `conversation-layering-conventions.md`. |
| !11528 | **Deep, merged master; authored and merged by John Thacker.** Fixes two-pass dissection of coalesced PROXY-header + child-protocol packets. The PROXY dissector remains bound to the outer TCP conversation while `conversation_set_conv_addr_port_endpoints()` establishes translated logical endpoints for child dissection; otherwise first-pass child binding could bypass PROXY on redissection. Promoted to `conversation-layering-conventions.md`. |
| !11527 | Merged master GSM A DTAP value-string correction. Uses current 3GPP TS 24.008 semantics while retaining historical GSM meanings as annotations for now-reserved values. Strong standards-history practice, but no separate notebook rule needed. |
| !11526 | Merged. Adds manufacturer-table dumping via `tshark -G`; part of the manufacturer-data API/export series. |
| !11525 | Merged. Adds H.265 to video codecs. Protocol/media feature work. |
| !11524 | Merged stable backport of CQL ERROR-message parsing fix. |
| !11523 | Merged stable backport of CQL ERROR-message parsing fix. |
| !11522 | **Deep/corroboration, merged.** CQL NULL (`-1`) and zero-length map/set values still require metadata/type offsets to advance. Reinforces the existing parser-progress rule that exceptional/empty values must not leave the parser at the same logical position. |
| !11521 | Merged master CQL ERROR-message parsing fix. |
| !11520 | **Deep/corroboration, merged.** Replaces Telnet's duplicated array-index/special-case option handling with a lookup returning a `const tn_opt *` plus an explicit unknown-option descriptor, allowing sparse/high option numbers such as VMware's 232 without a huge array. Author reports 100,000 fuzz passes and reproduced unrelated failures on the baseline. Good table-dispatch and validation evidence. |
| !11519 | **Deep, merged master; authored and merged by João Valverde.** Makes `not in` follow Wireshark inequality semantics rather than naïvely compiling it as the Boolean complement of `in`; absence and multi-value fields make those forms non-equivalent. Promoted to `display-filter-set-semantics-conventions.md`. |
| !11518 | **Deep/corroboration, merged.** ASAN exposed a startup global-buffer overflow because a `bool` backing variable was passed to `prefs_register_bool_preference()`, whose contract at the time required `gboolean *`; one-byte logical storage was written through a four-byte API type. Strong corroboration of the existing backing-storage/API-contract rule in `c-type-conventions.md`. |
| !11517 | **Deep, merged master; authored and merged by João Valverde.** Corrects `all X in S` to quantify over every occurrence of multi-valued field `X`; the old implementation combined set elements incorrectly. Promoted alongside !11519 to `display-filter-set-semantics-conventions.md`. |
| !11516 | **Deep/corroboration, merged.** New Fortinet FGCP heartbeat dissector based on packet analysis across multiple HA configurations, with three representative captures attached and substantive maintainer review. Strong evidence for capture-backed validation of reverse-engineered/new dissectors. |
| !11515 | Merged. Corrected replacement/reopen of !11511 for generated WOWW definitions. The corrected MR merged after the earlier submission's issues were fixed. |
| !11514 | Merged automatic data/translation update. Generated-maintenance work; low architectural signal. |
| !11513 | Merged automatic data/translation update. Generated-maintenance work; low architectural signal. |

## Durable notebook updates

1. `conversation-layering-conventions.md` — commit `8180601c4c1fb62b8b620059b27af6f0d12afe81`
   - !11529: do not instantiate transport-specific conversation state merely to obtain transport-neutral facts.
   - !11528: keep a protocol mediator such as PROXY on the outer conversation across redissection and attach child state only after logical endpoint translation.

2. `cleanup-ownership-conventions.md` — commit `876509c18c01b18828c24ff9ff86e145203cec33`
   - Guy Harris's !11546: honor composite cleanup ownership; do not pre-free members already owned by a higher-level cleanup routine, and centralize multi-resource teardown for error paths.

3. `display-filter-set-semantics-conventions.md` — commit `6af69529456ad926469e2063c4afb6ef4dfd897b`
   - !11517/!11519: set membership, `all`, `not in`, inequality, absent fields, and multi-valued fields require explicit quantifier/absence semantics rather than scalar Boolean rewrites.

Strong corroborating findings retained in this ledger include !11518 (API backing-storage width), !11550 (incompatible duplicate filter abbreviations), !11537 (packet-scope dynamic allocation), !11535 (typed-item checker interpretation), !11522 (parser progress), !11520 (sparse lookup/fuzzing), and !11516 (representative captures for new dissectors).

## Frontier probe

After fixing the exact 50-MR batch, !11512 (`[Automatic update for 2023-07-23]`) was inspected only to verify that the corpus continues. It exists and is merged, but **was not counted as reviewed in this run**. It remains the next descending candidate absent newly scraped higher-numbered unreviewed material.

The corpus therefore has not run out, and no scraper restart is required.
