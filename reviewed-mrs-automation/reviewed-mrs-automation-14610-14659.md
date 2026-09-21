# Wireshark MR automation review: !14610–!14659

- Reviewed on: 2026-09-21
- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit: `1c7ad509887ee25079a7865cc62ba18cba06f49a`
- Notebook starting commit: `d88905544d215a649dab6293291fb882767fa866`
- Reviewed in this run: **50 MRs**
- Outcome mix: **49 merged, 1 closed/unmerged (!14636)**

## Selection and duplicate avoidance

Before selecting this batch, the reviewed set was reconstructed from the available tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md`, the per-run files in `reviewed-mrs-automation/`, the immediately preceding exact ledger `reviewed-mrs-automation-14660-14709.md`, and the high-numbered ledger `reviewed-mrs-automation-14710-14748-plus-26567-26577.md`. The historical !17571–!17620 batch remains explicitly part of the reviewed set and was preserved/counting during selection.

The corpus commit is unchanged from the preceding run, and the higher !26567–!26577 arrivals in this snapshot were already recorded by an earlier exact ledger. Exact subtraction therefore yields the next fifty highest-numbered unreviewed corpus MRs as **!14659 through !14610 inclusive**. No numeric range was assumed reviewed merely because adjacent MRs appeared in a ledger.

Exact MRs reviewed in this run:

`!14659, !14658, !14657, !14656, !14655, !14654, !14653, !14652, !14651, !14650, !14649, !14648, !14647, !14646, !14645, !14644, !14643, !14642, !14641, !14640, !14639, !14638, !14637, !14636, !14635, !14634, !14633, !14632, !14631, !14630, !14629, !14628, !14627, !14626, !14625, !14624, !14623, !14622, !14621, !14620, !14619, !14618, !14617, !14616, !14615, !14614, !14613, !14612, !14611, !14610`

## Per-MR review ledger

| MR | Outcome / depth | Review finding |
| --- | --- | --- |
| !14659 | Merged / scanned | Automatic master data update for manufacturer/services/enterprise-number/translations data. Generated/reference-data maintenance; no new engineering convention. |
| !14658 | Merged / corroborating | Release-4.2 counterpart of the automatic data update. No independent lesson beyond generated/reference-data maintenance. |
| !14657 | Merged / corroborating | Release-4.0 counterpart of the automatic data update. No independent lesson. |
| !14656 | Merged / corroborating | Release-3.6 counterpart of the automatic data update. No independent lesson. |
| !14655 | Merged / focused | John Thacker adds the fixed WNM dialog token for action types whose variable portion is still undecoded, allowing a truthful Undecoded result instead of a false Malformed diagnosis. Corroborates the existing diagnostic rule that unsupported structure is not automatically malformed input. |
| !14654 | Merged / corroborating | Release-3.6 backport of the ASAP `pinfo` correctness fix from !14651. Stable propagation only. |
| !14653 | Merged / corroborating | Release-4.0 backport of the ASAP `pinfo` correctness fix. Stable propagation only. |
| !14652 | Merged / corroborating | Release-4.2 backport of the ASAP `pinfo` correctness fix. Stable propagation only. |
| !14651 | Merged / deep, high-authority follow-up | Gerald Combs fixes the ASAP path so `pinfo` is valid. John Thacker later points out that the real semantic intent is probably to suppress normal tap handling for error-cause packets and suggests `pinfo->flags.in_error_pkt`; Gerald implements that refinement in already-reviewed !14664. Retained as historical evidence, with !14664 weighted as the stronger final design. |
| !14650 | Merged / corroborating | Release-3.6 backport of BACnet/CIP/IPP/LISP/NETLINK recursion checks. Existing bounded-recursion guidance already covers it. |
| !14649 | Merged / corroborating | Release-4.0 counterpart of the same recursion hardening. No independent rule. |
| !14648 | Merged / corroborating | Release-4.2 counterpart of the same recursion hardening. No independent rule. |
| !14647 | Merged / corroborating | Release-3.6 ASAP/iSCSI/JXTA/MBIM/MQTT-SN recursion hardening. Existing recursion guidance applies. |
| !14646 | Merged / corroborating | Release-4.0 counterpart of the same recursion hardening. |
| !14645 | Merged / corroborating | Release-4.2 counterpart of the same recursion hardening. |
| !14644 | Merged / focused | Gerald Combs's master BACnet/CIP/IPP/LISP/NETLINK recursion hardening. Strong historical support for the already-recorded explicit dissection-depth model; no duplicate notebook rule added. |
| !14643 | Merged / focused | Gerald Combs's master ASAP/iSCSI/JXTA/MBIM/MQTT-SN recursion hardening. Corroborates the same bounded-recursion model. |
| !14642 | Merged / focused | John Thacker fixes IEEE 802.11 SID display endianness: extraction already used little-endian, so the protocol-tree item must use the same encoding or tree and derived values disagree. Corroborates existing field-representation/encoding consistency guidance. |
| !14641 | Merged / corroborating | Release-3.6 AMQP/GNW/LDP/CFLOW/RDP recursion hardening. No new convention. |
| !14640 | Merged / corroborating | Release-4.0 counterpart of the same recursion hardening. |
| !14639 | Merged / corroborating | Release-4.2 counterpart of the same recursion hardening. |
| !14638 | Merged / focused | Gerald Combs's master AMQP/GNW/LDP/CFLOW/RDP recursion hardening. Existing recursion conventions already cover the accepted pattern. |
| !14637 | Merged / focused | IPv6 IOAM permits `nodelen == 0` when only Opaque State Snapshot is present; the parser must not reject a protocol-valid zero-length case merely because a previous safety check assumed positive length. Corroborates existing empty-value and protocol-validity guidance. |
| !14636 | Closed/unmerged / down-weighted | Earlier submission of the same IOAM zero-length correction, closed before merge and superseded by merged !14637. Used only as historical context. |
| !14635 | Merged / scanned | Spelling cleanup plus checker/dictionary maintenance. Useful repository hygiene but no new cross-cutting convention extracted. |
| !14634 | Merged / focused | PROFINET PROFIenergy addition. Martin Mathieson catches `val_to_str(..., "Unknown")` without a format specifier and recommends a suitable format or `_const()`; the contributor fixes it before merge. Corroborates checker/API-contract discipline for value-string helpers. |
| !14633 | Merged / corroborating | Release-3.6 backport of broad recursion checks. Existing recursion guidance applies. |
| !14632 | Merged / corroborating | Release-4.0 counterpart of the same broad recursion hardening. |
| !14631 | Merged / corroborating | Release-4.2 counterpart of the same broad recursion hardening. |
| !14630 | Merged / deep | John Thacker adds Z21 heuristics because the protocol uses unregistered UDP ports and unconditional port binding would dissect unrelated traffic. The accepted change validates packet structure instead of trusting the port alone; review also corrected X-Bus naming consistency. Strong corroboration of existing dissector-detection/heuristic guidance. |
| !14629 | Merged / focused | John Thacker handles an observed zero-length VMware heartbeat address. Alexis La Goutte asks for a reproducer; John explains it was found through 4.0-vs-4.2 regression testing and that the rest of the protocol signature is valid. Reinforces real-capture regression validation and legitimate empty-field handling. |
| !14628 | Merged / focused | Gerald Combs's master broad recursion checks. Strong historical support for bounded recursive dissection, already represented by later reviewed work. |
| !14627 | Merged / corroborating | Release-3.6 backport of the central `pinfo->dissection_depth` recursion API. |
| !14626 | Merged / corroborating | Release-4.0 backport of the central dissection-depth API. |
| !14625 | Merged / corroborating | Release-4.2 backport of the central dissection-depth API. |
| !14624 | Merged / focused | John Thacker fixes IEEE 802.11 AID masking and places SA in the source rather than destination column. Corroborates existing packet-address semantic ownership and packed-field correctness rules. |
| !14623 | Merged / deep | GSMTAP version dispatch. Anders Broman explicitly questions whether unknown versions should still be decoded; the contributor explains future GSMTAP layouts need not be v2-compatible. Final code identifies GSMTAP, exposes the unknown version with an expert warning, and stops before v2-specific body decoding. Added to `protocol-implementation-compatibility-conventions.md`. |
| !14622 | Merged / scanned | Falcodump manpage improvement documenting CloudTrail usage. Documentation-only, no new engineering convention. |
| !14621 | Merged / deep | John Thacker replaces one gRPC handle that inferred its call mode from `pinfo->match_string` with separate gRPC/gRPC-Web/gRPC-Web-Text handles calling shared logic; this also makes Decode-As and stream-ID invocation safe. Strong independent corroboration of Guy Harris's later, higher-authority !26224/!26229 rule against guessing a dissector data/call contract from incidental dispatch metadata. No duplicate notebook rule added. |
| !14620 | Merged / corroborating | Release-4.2 backport of `make-services.py` range parsing. Confirms the generator regression affected generated service tables. |
| !14619 | Merged / focused | John Thacker fixes `make-services.py` so a source range contributes every port, not merely the endpoints. Corroborates existing generated-data source-of-truth/testing guidance. |
| !14618 | Merged / focused | TWAMP sender timestamp format is governed by the Sender Error Estimate Z bit at `offset + 8`, not the timestamp start. Protocol-specific offset/flag correction; no general rule beyond existing field-offset discipline. |
| !14617 | Merged / focused | John Thacker extends MEGACO audit items to descriptors as permitted by H.248 v3. Protocol feature/correctness change; no new cross-cutting convention. |
| !14616 | Merged / deep | Anders Broman moves Windows builds to Lua 5.3. John Thacker catches shared test syntax using Lua 5.3-only floor division even though older Lua remains supported elsewhere, and recommends a version-neutral `math.floor` form. Added to `dependency-version-compatibility-conventions.md`: a per-platform upgrade does not silently raise the repository-wide runtime syntax floor. |
| !14615 | Merged / deep | Long-reviewed HTTP request/response matching fix. Ordered asynchronous requests must be correlated FIFO, not LIFO. John Thacker also finds nullable correlation state, leaked `GSList` state, pointless allocations, fragile parsing, and validity flags set after failed parsing; he verifies leaks with Valgrind and LeakSanitizer on fuzz input and recommends wmem where appropriate. Added to `transaction-matching-conventions.md`. |
| !14614 | Merged / focused | John Thacker finds MEGACO audit tokens when the audit item is itself a descriptor. Protocol-specific parser completeness. |
| !14613 | Merged / focused | John Thacker fixes MEGACO stream-id item length by avoiding double-counting `tokenlen` already incorporated into `tvb_offset`. Corroborates existing cursor/consumption accounting guidance. |
| !14612 | Merged / deep | Gerald Combs introduces the central `packet_info.dissection_depth` API and uses `gui_max_tree_depth` across dispatcher/heuristic recursion paths. High-value historical source for recursion architecture, but later reviewed recursion MRs already capture the durable convention. |
| !14611 | Merged / scanned | Restores CMake's default `INSTALL_DATADIR` and installs Logray data beneath its own subdirectory. Build/install layout maintenance; no broader rule extracted. |
| !14610 | Merged / focused | John Thacker makes Asciidoctor tagged includes explicit enough to work on older supported tooling such as RHEL 8's version. Corroborates build/documentation portability: do not rely on newly implied defaults when older supported tools require explicit syntax. |

## Notebook updates from this run

- `transaction-matching-conventions.md` — added FIFO matching for ordered protocols with multiple outstanding requests; correlation state is valid only after successful parse, and persistent tracking structures need explicit lifetime/leak testing (!14615).
- `protocol-implementation-compatibility-conventions.md` — added the explicit compatibility judgment for unknown wire versions: where future layout is not guaranteed compatible, retain minimal identification/version diagnostics and stop before known-version body decoding (!14623).
- `dependency-version-compatibility-conventions.md` — added the rule that upgrading one platform's runtime does not raise the shared source/test syntax floor above the oldest still-supported runtime (!14616).

Other strong findings were deliberately retained as corroboration rather than duplicated: !14621 independently confirms the later Guy Harris !26224/!26229 separate-entry-point architecture; !14630 reinforces heuristic detection on unregistered/shared ports; !14651 is superseded architecturally by already-reviewed !14664; and the large Gerald Combs recursion series reinforces existing bounded-recursion guidance.

## Continuation

`mr_14609.json` exists at the same corpus commit and is merged, so the corpus is not exhausted. Absent newly scraped higher-numbered unreviewed MRs in a future corpus snapshot, !14609 is the next descending candidate.