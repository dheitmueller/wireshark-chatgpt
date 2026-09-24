# Reviewed Wireshark merge requests: !11263–!11312

- **Corpus commit reviewed:** `ddcaa22b51c68f594e425a23388c3a2086813054`
- **Review direction:** descending MR number, newest available previously-unreviewed first
- **Exact MR count:** 50
- **Exact selected set:** !11312, !11311, !11310, !11309, !11308, !11307, !11306, !11305, !11304, !11303, !11302, !11301, !11300, !11299, !11298, !11297, !11296, !11295, !11294, !11293, !11292, !11291, !11290, !11289, !11288, !11287, !11286, !11285, !11284, !11283, !11282, !11281, !11280, !11279, !11278, !11277, !11276, !11275, !11274, !11273, !11272, !11271, !11270, !11269, !11268, !11267, !11266, !11265, !11264, !11263
- **Tracking consulted before selection:** `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, and the available per-run files under `reviewed-mrs-automation/`, including the immediately preceding `reviewed-mrs-automation-11313-11362.md`.
- **Historical tracking preserved:** the previously reviewed !17571–!17620 batch remains part of the already-reviewed set.
- **Prior frontier probe:** !11312 had only been inspected to establish the previous run's frontier and was not previously counted as reviewed.
- **Status mix:** 45 merged, 4 closed/unmerged (!11312, !11302, !11282, !11275), and 1 open in the corpus snapshot (!11311).

Merged master changes are primary evidence. Stable-branch backports primarily corroborate accepted behavior. Closed, open, abandoned, and superseded proposals are down-weighted. Maintainer-authored or maintainer-approved changes—especially Guy Harris and John Thacker—receive correspondingly high weight.

## Exact review ledger

| MR | State | Review result |
|---|---|---|
| !11312 | closed | Down-weighted. DHCPFO empty message-digest proposal was closed by its author because !11336 handled the case as well; useful only as superseded history. |
| !11311 | open snapshot | Down-weighted/provisional. Proposed validation of corrupt dumpcap sync-pipe messages. Guy Harris identified an architectural cause: third-party capture libraries may write human stderr into the channel and argued that dumpcap stderr should ideally travel on a separate pipe from structured control/data. Valuable design discussion, but not accepted code in this corpus snapshot. |
| !11310 | merged | Display-filter memory-cell refactor replaces a GSList-based register representation with a dedicated typed cell object. Useful internal type/locality cleanup; no distinct new notebook rule. |
| !11309 | merged | Adds Unix-time support for absolute-time fields across the filter/value stack. Broad accepted feature, but no new general convention beyond existing time-field guidance. |
| !11308 | merged | Falcodump/Falco bridge update for newer libsinsp APIs. Dependency/API maintenance; no durable new Wireshark-wide rule extracted. |
| !11307 | merged | CAN/FlexRay/LIN configurable source/destination mapping uses validated UAT records plus a composite bus/channel/cycle/frame key. Corroborates complete semantic keys and explicit copy/free/lifetime handling. |
| !11306 | merged | Include-path cleanup. Pascal Quantin recommended local `"packet-socketcan.h"` inclusion rather than spelling the repository path; useful include-style corroboration. |
| !11305 | merged | Automatic master update. No review-derived convention. |
| !11304 | merged | Automatic release-4.0 update. No review-derived convention. |
| !11303 | merged | Automatic release-3.6 update. No review-derived convention. |
| !11302 | closed | Down-weighted. Proposed release-4.0 ACDR type additions were explicitly identified by the author as an enhancement; Alexis La Goutte stated stable backports are for fixes, not enhancements. Corroborates existing backport-scope policy. |
| !11301 | merged | DHCPFO interoperability workaround for a draft's anomalous payload offset. Protocol-specific compatibility handling; no general rule promoted. |
| !11300 | merged | RTP RFC 5285 header-extension value-table addition. Straightforward field/presentation enhancement. |
| !11299 | merged | RTP stream-selection fixes account for multiple SSRCs sharing address/port tuples under BUNDLE and avoid duplicate internal stream records/double-free risk. Corroborates semantic stream identity and ownership discipline. |
| !11298 | merged | Documentation/help URL fixes. No durable coding convention. |
| !11297 | merged | NAS 5GS URSP traffic-descriptor additions. Protocol enhancement; no general rule extracted. |
| !11296 | merged | Regenerates Skinny from its generator source and fixes generated field width/masks. Review raised making regeneration mechanically available in the build/container environment; corroborates existing generated-code/checker guidance. |
| !11295 | merged | Display-filter arithmetic expressions in set elements. Accepted parser feature; no distinct rule beyond existing display-filter semantics guidance. |
| !11294 | merged | `to_str` C-style cleanup. Routine modernization. |
| !11293 | merged | AMR enum definitions. Local codec cleanup. |
| !11292 | merged | AMR reserved-bit handling now actually falls back to bandwidth-efficient mode as the expert text claimed. Corroborates the rule that diagnostics must describe real parser behavior. |
| !11291 | merged | Deep/high weight. John Thacker-authored and merged codec-registry fix applies RFC 4855 case-insensitive RTP encoding-name semantics consistently to registration and lookup. Promoted to `dissector-table-registration-conventions.md`. |
| !11290 | merged | John Thacker-authored/merged cleanup uses the named `STRING_CASE_INSENSITIVE` mode rather than a bare Boolean in the string-table implementation. Reinforces explicit semantic mode constants; promoted with !11291. |
| !11289 | merged | Deep/high-authority corroboration. Guy Harris authored and approved the change, grouping `print_line_color` with `print_line` and documenting constructor versus Boolean print-operation return contracts. Reinforces explicit public API return semantics. |
| !11288 | merged | QUIC ACK_FREQUENCY draft update included a representative capture in discussion and updated protocol fields/constants to the newer draft. Strong submission/testing corroboration, not a separate rule. |
| !11287 | merged | Stable-branch backport of fuzz-CI retry policy. Corroboration only. |
| !11286 | merged | Stable-branch backport of fuzz-CI retry policy. Corroboration only. |
| !11285 | merged | Master CI change disables automatic retries for fuzz jobs. Focused CI policy; no broader rule inferred without more discussion. |
| !11284 | merged | Debian symbol-list maintenance. Packaging-only change. |
| !11283 | merged | High-authority corroboration. Adds the direct `<errno.h>` dependency after a compiler failure; Guy Harris approved. Supports explicit/self-contained include dependencies and the own-header-first validation rule. |
| !11282 | closed | Down-weighted. Proposed conversion of pcapng `if_fcslen` bit units to Ethernet-dissector byte units; failed pipeline and closed without maintainer discussion in the snapshot. Not used as accepted evidence. |
| !11281 | merged | IEEE 1905 typo corrections. No general lesson. |
| !11280 | merged | John Thacker-approved/merged Wiretap fix permits `WTAP_TSPREC_UNKNOWN` when generating synthetic IDBs and chooses a default microsecond unit instead of asserting. Corroborates treating legitimate unknown/absent metadata as data-domain state rather than an impossible invariant. |
| !11279 | merged | G.729 Annex B SID/CN decoding support, with explicit scope note that missing-frame concealment would require broader RTP audio-stream work. Useful scope discipline, but codec-specific. |
| !11278 | merged | John Thacker-authored direct `<errno.h>` additions after common headers stopped supplying it transitively; Rocky Linux exposed the hidden dependency. Corroborates direct-include and header self-containment guidance. |
| !11277 | merged | Converts CRC utility interfaces to C99 integer types. Type-modernization corroboration. |
| !11276 | merged | Falcodump AWS-region registry update. Data maintenance only. |
| !11275 | closed | Down-weighted. Wiretap-wide own-header-first proposal was repeatedly rebased and ultimately closed with an empty final diff. Its intent corroborates !11274 but is not accepted evidence itself. |
| !11274 | merged | Deep. Root implementation files include their corresponding header first specifically to expose headers that do not include their own dependencies. Promoted to `public-header-validation-conventions.md`, with !11278/!11283 as concrete corroboration. |
| !11273 | merged | release-4.0 IO Graph fix keeps packet selection in the correct time domain when Time of Day display is enabled. UI correctness backport; no new cross-cutting rule. |
| !11272 | merged | Falcodump label/tooltip correction. UI text maintenance only. |
| !11271 | merged | release-3.6 Qt context-menu shortcut workaround. Backport corroboration only. |
| !11270 | merged | release-4.0 Qt context-menu shortcut workaround. Backport corroboration only. |
| !11269 | merged | Master Qt workaround explicitly resets `AA_DontShowShortcutsInContextMenus` to defeat the affected Qt behavior. Framework-specific compatibility fix. |
| !11268 | merged | Deep/high weight. HTTP/2 moves from private streaming reassembly back to the enhanced common `reassemble_streaming_data_and_call_subdissector()` framework; adds a public completion-state query and two-pass regression coverage. John Thacker approved and merged. Promoted to `reassembly-conventions.md`. |
| !11267 | merged | release-3.6 backport of OpenFlow v1 datapath-ID field-order correction. Backport corroboration. |
| !11266 | merged | release-4.0 backport of OpenFlow v1 datapath-ID field-order correction. Backport corroboration. |
| !11265 | merged | OpenFlow v1 `OFPT_FLOW_REMOVED` support, merged by John Thacker. Protocol enhancement. |
| !11264 | merged | Master OpenFlow v1 datapath-ID field-order correction, merged by John Thacker. Focused correctness fix. |
| !11263 | merged | Martin Mathieson-authored and merged migration to `proto_tree_add_item_ret_uint()` helpers in additional call sites. Reinforces existing typed-item/helper guidance rather than adding another overlapping rule. |

## Durable notebook updates

- **!11291 + !11290 — protocol-defined text equality must cover both registry insertion and lookup.** RTP codec names are case-insensitive by specification, so the accepted registry implementation uses case-insensitive matching end-to-end and names that mode explicitly. Added to `dissector-table-registration-conventions.md` in commit `1efa73b5730eebf08fdb66444baa1fddd68db852`.
- **!11274 with !11278/!11283 — include an implementation's own header first to expose hidden transitive dependencies.** The merged root-wide include ordering deliberately validates header self-containment; adjacent `errno.h` fixes demonstrate the failure mode. Added to `public-header-validation-conventions.md` in commit `1f20db2e990c4d431b4785e1ab24e93af9bf86f4`.
- **!11268 — consolidate duplicate streaming-reassembly logic in the common framework and expose framework state through public queries.** HTTP/2 moved back to the generic helper and its affected tests explicitly use two-pass dissection. Added to `reassembly-conventions.md` in commit `e7c23d61815d681483d8d9ee6f62589837f48bbb`.

## Down-weighted or superseded evidence

- **!11312** is superseded by !11336 and was not merged.
- **!11311** contains useful Guy Harris architectural discussion but remained open in the corpus snapshot, so it is not treated as an accepted implementation rule.
- **!11302** was explicitly classified as an enhancement inappropriate for a stable-branch backport and was closed.
- **!11282** was closed with a failed pipeline and no substantive maintainer acceptance in the snapshot.
- **!11275** was ultimately closed with an empty final diff; the merged !11274 is the accepted evidence for the same own-header-first technique.

## Frontier check

MR **!11262** (`AMR: Support RTP payloads with multiple frames`) exists in the same corpus commit and is merged. It was inspected only to verify that the corpus continues below this batch; **it is not counted among these 50 reviewed MRs** and remains eligible for the next descending run.