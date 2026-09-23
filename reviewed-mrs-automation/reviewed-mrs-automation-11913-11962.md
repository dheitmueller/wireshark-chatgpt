# Wireshark MR review automation: !11913–!11962

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Selection method: reconstructed the already-reviewed set from `reviewed-mrs.md`, the aggregate automation tracker, and the available per-run files under `reviewed-mrs-automation/`, including the previously reviewed !17571–!17620 historical batch. The prior run's mention of !11962 was only a frontier check and was not counted as reviewed. The exact fifty highest-numbered corpus MRs not found in the reviewed set were !11962 down through !11913. No numeric interval was assumed reviewed merely from partial ledger coverage.

Weighting: merged master work received the highest implementation weight; merged release backports were treated mainly as corroboration of their master changes; the one closed/unmerged draft was down-weighted. Substantive maintainer reasoning was weighted independently, with Guy Harris's time/portability analysis treated as especially authoritative.

| MR | Status / review weight | Review note |
| --- | --- | --- |
| !11962 | Merged master / corroborating | Martin Mathieson fixes multiple label/filter-name and field-registration copy/paste mistakes. Reinforces treating checker-discovered labels, abbreviations, masks, and value tables as semantic metadata rather than cosmetic strings. |
| !11961 | Merged master / build maintenance | Gerald Combs updates macOS dependency versions. The MR was temporarily held as draft for an unreleased Qt version and then trimmed until the dependency actually existed, a useful example of keeping setup manifests tied to released artifacts rather than anticipated versions. |
| !11960 | Merged master / correctness | John Thacker moves initialization of `prev_dis` inside the `passed_dfilter` path so state named "previous displayed packet" is updated only by displayed packets. Good state-invariant example; no separate notebook rule added. |
| !11959 | Merged master / checker cleanup | Martin Mathieson corrects several `value_string` entries discovered by audits, including missing/reserved entries and copy/paste labels. Corroborates existing value-table/checker conventions. |
| !11958 | Merged master / UI correctness | Fixes `register_packet_menu()` submenu grouping by passing the shared `QHash` by reference instead of copying it for each recursive call. Straightforward shared-state/copy-semantics fix. |
| !11957 | Merged master / protocol support | Adds GB/T 38636 ClientKeyExchange handling for the SM2 cipher suites and supplies a capture for validation. Useful new-protocol-change evidence: cite the defining standard and provide a representative capture. |
| !11956 | Merged master / API documentation | Clarifies WSLua `register_packet_menu()` callback arguments and required-field behavior so the documentation matches the actual callback contract. |
| !11955 | Merged master / documentation | Grammar/documentation cleanup for WSLua `new_dialog()`; no durable implementation rule. |
| !11954 | Merged master / deep portability evidence | Enabling the system `timegm()` exposed macOS/FreeBSD behavior where a successful conversion could leave `errno=EOVERFLOW`. Guy Harris traced the failure and documented that `timegm()` did not provide the assumed `errno` contract. Added as origin/corroboration in `errno-contract-conventions.md`; the later !11971/!11973/!11998/!12002 fixes remain the stronger final API precedent. |
| !11953 | Merged master / cleanup | Indentation-only cleanup in `wsutil/to_str.c`; no durable rule. |
| !11952 | Merged master / checker maintenance | `check_tfs.py` catches remaining true/false-string cases whose two strings match exactly. Corroborates using static checker findings to expose copy/paste field-metadata errors. |
| !11951 | Merged master / deep parser corroboration | Extends display-filter time-zone parsing after the deterministic UTC fix. Used with !11947 in `time-parsing-conventions.md`: accepted time-zone syntax should have portable, unambiguous semantics rather than depend on host abbreviation guessing. |
| !11950 | Merged master / analyzer cleanup | Makes Thrift field-header initialization explicit enough for Clang analyzer despite the existing control-flow argument that the values are overwritten before legitimate reads. Useful analyzer-maintenance evidence, but no broader convention promoted. |
| !11949 | Merged master / new dissector | Adds VMware HeartBeat dissection. Review included `tools/fix-encoding-args.pl` findings and iterative readiness cleanup before merge, reinforcing the established new-dissector/checker workflow. |
| !11948 | Merged master / parser cleanup | Refactors display-filter time-literal parsing around the same time-zone work. Weighted as implementation support for the accepted parser direction rather than a separate rule. |
| !11947 | Merged master / deep, Guy Harris discussion | Adds `UTC` suffix parsing. In substantive discussion Guy explains why arbitrary abbreviations such as CEST/PST/WET should not be accepted generically: abbreviation-to-zone mapping is ambiguous and there is no single portable conversion API across supported UNIX/Windows systems. Promoted to `time-parsing-conventions.md`. |
| !11946 | Merged master / protocol cleanup | Adds reserved PTP L1Sync bitfields so the packet layout is represented completely. Protocol-specific completeness improvement. |
| !11945 | **Closed/unmerged draft / low weight** | Proposed a PTP TLV display-format backport. Alexis La Goutte questioned whether the enhancement belonged in the stable branch; it was closed because the fix/enhancement would ship in the next 4.2.x line. Retained only as backport-policy evidence, not implementation precedent. |
| !11944 | Merged master / correctness | Fixes E2AP copy/paste errors in new `ranFunction` method handling. Straightforward generated/manual dispatch correction. |
| !11943 | Merged master / cleanup | Spelling fixes only. |
| !11942 | Merged release backport / lower independent weight | Backport of the Tibia encoding-argument correction from !11938. Corroborates the master rule. |
| !11941 | Merged release backport / lower independent weight | Another stable-branch backport of !11938; corroborating only. |
| !11940 | Merged master / protocol support | Adds GB/T 38636 ServerKeyExchange handling for the same SM2 suite family. Part of the accepted TLS support series; no general architecture rule beyond standards-driven dispatch coverage. |
| !11939 | Merged master / protocol/checker cleanup | IEEE 802.11 element updates plus warning fixes. Useful checker/protocol-maintenance evidence, but no new durable convention. |
| !11938 | **Merged master / deep, Guy Harris authored** | Fixes Tibia calls that supplied negotiated string-encoding flags to non-string fields; Guy states that Wireshark does not guarantee such use works, and it did not. Promoted to `field-decoding-api-conventions.md`: encoding flags must match the registered field type; use `ENC_NA` or the appropriate byte order for non-string items. |
| !11937 | Merged release backport / lower independent weight | Stable-branch CI backport switching to Clang 16; master policy is weighted at !11935. |
| !11936 | Merged release backport / lower independent weight | Additional stable-branch Clang 16 CI backport; corroborating only. |
| !11935 | Merged master / CI maintenance | Moves GitLab CI to Clang 16. Toolchain-version maintenance, with no separate durable rule beyond existing CI configuration guidance. |
| !11934 | Merged master / feature | Adds TTL/Hop Limit stats-tree support. Focused feature addition; no general convention extracted. |
| !11933 | Merged master / robustness | Initializes HTTP/3 state called out by Coverity. Straightforward uninitialized-state correctness fix. |
| !11932 | Merged master / build correctness | Fixes a copy/paste `endif` variable around `NGHTTP3_FOUND`. Build-system correctness cleanup. |
| !11931 | Merged master / portability | Introduces/uses the portable `ws_strptime_p()` selection path so system `strptime()` remains preferred where available and the internal implementation remains the fallback, preserving existing behavior. Corroborates keeping platform wrappers explicit rather than silently changing parser semantics. |
| !11930 | Merged master / cleanup | Removes an unnecessary include and makes a PTP variable static. Scope/build hygiene only. |
| !11929 | Merged master / tooling | Martin Mathieson updates `check_typed_item_calls.py` and fixes concrete field-registration issues discovered by the checker. Reinforces the established practice of treating checker output as semantic review input. |
| !11928 | Merged automated update / low semantic weight | Scheduled generated/update content; no durable coding convention. |
| !11927 | Merged automated update / low semantic weight | Scheduled generated/update content; no durable coding convention. |
| !11926 | Merged automated update / low semantic weight | Scheduled generated/update content; no durable coding convention. |
| !11925 | Merged master / portability cleanup | Fixes a PTP `printf` format mismatch. Corroborates existing format/type portability guidance. |
| !11924 | Merged master / test hygiene | Avoids emitting empty debug log records in display-filter tests by checking captured stderr before logging it. Test-output hygiene, not a broader architecture rule. |
| !11923 | Merged master / logging API refactor | Refactors wslog around a message manifest and adds a portability wrapper for local time conversion. Substantial internal API cleanup, but this batch did not provide enough external review discussion to promote a separate convention. |
| !11922 | Merged release backport / documentation | Backport of editcap `-A`/`-B` documentation clarification. Lower independent weight. |
| !11921 | Merged release backport / documentation | Another stable-branch backport of editcap `-A`/`-B` documentation clarification. |
| !11920 | Merged release backport / corroborating | Release-3.6 backport of Guy Harris's `unix_epoch_to_nstime()` fix from !11917. Corroborates direct numeric epoch parsing. |
| !11919 | Merged master / documentation | Improves editcap `-A`/`-B` documentation; master source for the later backports. No coding convention. |
| !11918 | Merged release backport / corroborating | Stable-branch backport of !11917's Unix-epoch parser fix. |
| !11917 | **Merged master / deep, Guy Harris authored** | Rewrites `unix_epoch_to_nstime()` to parse epoch seconds/fractions directly with integer helpers, range-check the conversion to `time_t`, and stop using non-standard `strptime("%s")`/`struct tm` machinery that can import local-time semantics. Promoted to `time-parsing-conventions.md`. |
| !11916 | Merged master / tests | Modernizes display-filter assertions. Test-code cleanup; no new durable rule. |
| !11915 | Merged master / tooling | Corrects a `check_tfs.py` error message. Tooling polish only. |
| !11914 | Merged master / protocol support | Telnet AUTHENTICATION now dispatches according to the negotiated authentication type instead of assuming Kerberos 5, adds SSL handling, cites the best available implementation reference, and reports 1000 fuzz iterations. Good protocol-evolution/testing evidence; no universal rule beyond established dispatch and fuzzing guidance. |
| !11913 | Merged master / protocol support | Extends TLS key-exchange algorithm selection for GB/T 38636 cipher suites, forming the base of the !11940/!11957 support series. Standards-driven mapping change. |

## Durable notebook updates from this run

- Added `time-parsing-conventions.md` from !11917/!11918/!11920 and !11947/!11951: parse numeric epoch values directly in their native numeric domain; accept time-zone syntax only when its meaning can be made portable and unambiguous.
- Extended `field-decoding-api-conventions.md` from Guy Harris's !11938 plus !11941/!11942: `ENC_*` flags must match the registered field type; negotiated string encodings do not belong on non-string fields.
- Extended `errno-contract-conventions.md` with !11954 as the high-authority platform-origin case: enabling an available libc implementation does not imply its incidental `errno` behavior matches Wireshark's fallback or caller assumptions.

Strong corroboration intentionally not promoted into duplicate rules includes checker-driven label/value-string cleanup (!11962, !11959, !11952, !11929), representative-capture/new-dissector workflow evidence (!11957, !11949), display-state correctness (!11960), and the lower-weight stable-branch backports noted above.

Frontier check only (not reviewed): !11912, `Remove some uses of tvb_new_subset_length_caplen()`, exists in the corpus at the same commit and is merged. It remains the next descending candidate absent newly scraped higher-numbered unreviewed material.
