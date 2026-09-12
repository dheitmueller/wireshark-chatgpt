# Wireshark MR review batch: !22692–!22642, excluding previously reviewed !22662

Corpus commit used: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Exactly 50 previously unreviewed corpus MRs were reviewed, newest to oldest. The already-reviewed set was rebuilt from `reviewed-mrs.md` and the per-run files under `reviewed-mrs-automation/`; the historical !17571–!17620 batch was preserved and counted. !22662 was already individually reviewed and therefore was not reviewed again.

Exact reviewed MR set, in review order:

!22692, !22691, !22690, !22689, !22688, !22687, !22686, !22685, !22684, !22683, !22682, !22681, !22680, !22679, !22678, !22677, !22676, !22675, !22674, !22673, !22672, !22671, !22670, !22669, !22668, !22667, !22666, !22665, !22664, !22663, !22661, !22660, !22659, !22658, !22657, !22656, !22655, !22654, !22653, !22652, !22651, !22650, !22649, !22648, !22647, !22646, !22645, !22644, !22643, !22642.

## Review notes

| MR | Weight | Review result |
|---|---|---|
| !22692 | Deep / merged | John Thacker allocator-accounting cleanup checks allocator usable size where the platform exposes it. Useful memory-accounting implementation detail; no broader rule promoted. |
| !22691 | Scanned / merged | ASTERIX generator and UAP-selection feature/refactor. Review caught generator spelling issues and required a rebase; feature-specific. |
| !22690 | Scanned / merged | Adds an ACN SDT wrapped-protocol dissector table so device-specific DMP interpretation can live in plugins. Good extensibility example, but not promoted as a universal architecture rule. |
| !22689 | Discussion-focused / closed draft | John Thacker rejected a RADIUS dictionary type-name update because Wireshark's dictionary parser did not support the proposed FreeRADIUS type names and asked whether it had actually been tested. Down-weighted because it closed unmerged; retains the general lesson that external-format compatibility changes must be validated against the local parser. |
| !22688 | Corroboration / merged release backport | Observer writer fix backport for incorrect TLV sizing; same master fix as !22672. |
| !22687 | Corroboration / merged release backport | Observer writer fix backport for incorrect TLV sizing; same master fix as !22672. |
| !22686 | Scanned / merged | Null-checks the result of preference lookup before use; straightforward Coverity correctness fix. |
| !22685 | Scanned / merged | Corrects an 802.11 test to use the fixture and key template it claims to exercise; useful test-integrity cleanup without a new notebook rule. |
| !22684 | Scanned / merged | Makes a PROCMON-local function static; ordinary linkage hygiene. |
| !22683 | Scanned / merged | MIDI/SysEx naming cleanup preparing shared MIDI functionality; architecture-specific refactor. |
| !22682 | Corroboration / merged | Uses `intmax_t`/`uintmax_t` for `time_t` formatting to avoid 32-bit/Y2038 assumptions; consistent with existing C type/portability guidance. |
| !22681 | Scanned / merged | UMTS RLC initialization cleanup for Coverity; no distinct convention. |
| !22680 | Scanned / merged | Moves dead TODO code under its existing `#if 0` guard instead of compiling inert scaffolding. |
| !22679 | Scanned / merged | Adds `get_data_source_name()` for API completeness; small API addition. |
| !22678 | Scanned / merged | Qt response-time display correctly handles both rtd-table layouts; feature correctness rather than reusable review policy. |
| !22677 | Scanned / merged | Replaces a raw print with `ws_error()` in epan; consistent error-reporting cleanup. |
| !22676 | Scanned / merged | Corrects constness of a preference string return value. |
| !22675 | Scanned / merged | Fixes registrar field-count dumping to respect the reserved index-0 entry. |
| !22674 | Corroboration / merged | Checks `lua_next()` return value and treats unexpected failure as failure; reinforces return-value checking. |
| !22673 | Corroboration / merged | Removes a datestamp from generated BACnet vendor output so unchanged inputs do not create weekly content churn; reinforces deterministic generated output. |
| !22672 | Deep / merged master | John Thacker fixes Observer TLV serialization by keeping the TLV header separate from the value structure and using the actual value size. Strong serialized-layout evidence; later notebook rules already cover wire-format size versus C structure layout. |
| !22671 | Deep / merged | Fixes GIOP code that depended on C function-argument evaluation order while mutating shared offset state. Strong corroboration of `c-expression-sequencing-conventions.md`. |
| !22670 | Corroboration / merged | Propagates failures from ARCFour helper functions instead of returning success unconditionally; reinforces failure propagation. |
| !22669 | Scanned / merged | Strengthens const qualification of fixed RTPS strings, helping both correctness and static analysis. |
| !22668 | Scanned / merged | Initial RoMON identification by assigned EtherType; protocol feature addition. |
| !22667 | Scanned / merged | Corrects fallback to the proxy source port in 3GPP nettrace handling; targeted copy/paste correctness fix. |
| !22666 | Scanned / merged | Automated registry/data update with BACnet failure noted; generated-data maintenance only. |
| !22665 | Scanned / merged | Automated registry/data update; no durable human-review lesson. |
| !22664 | Scanned / merged | Automated registry/data update; no durable human-review lesson. |
| !22663 | Deep / merged | Removes `if (tree)` fences that prevented PTP TLV subdissectors from running on the first pass. Strong corroboration of the existing rule that tree-presence guards must not suppress semantic dissection or non-UI side effects. |
| !22661 | Deep / merged | Splits event dissector registration so Stratoshark loads only the dissectors it needs. Gerald Combs identified event dissectors that belong on the Stratoshark side; later testing exposed dependency assumptions in reduced registration. Useful frontend-separation evidence, but the subsequent fix series is better evidence than this transitional MR alone. |
| !22660 | Scanned / merged | Corrects HTTP/3 Quarter Stream range arithmetic (division/shift direction). Targeted protocol arithmetic fix. |
| !22659 | Scanned / merged | Fixes LTE RRC generated/dissection value-pointer handling for an NR report list. |
| !22658 | Scanned / merged | Moves SysEx code into the MIDI dissector to provide a shared home for MIDI functionality used by multiple transports. |
| !22657 | Scanned / merged | Initializes RTP Player dialog members flagged by Coverity. |
| !22656 | Scanned / merged | Initializes an OverlayScrollBar member to the correct default. |
| !22655 | Corroboration / merged | LTE RRC report-list fix related to !22659; no additional durable rule. |
| !22654 | Scanned / merged | Removes/fixes an effectively unused WiMAX variable hidden by `_U_`; static-analysis cleanup. |
| !22653 | Scanned / merged | Parallelizes `check_col_apis.py` with `concurrent.futures`; tooling performance improvement. |
| !22652 | Corroboration / merged | Adds a CMake target to rebuild CORBA IDL dissectors when `omniidl` is present; reinforces making generated code reproducible from authoritative inputs. |
| !22651 | Scanned / merged | Corrects UDP conversation behavior for RTP/T.38 identification; subsystem-specific conversation fix. |
| !22650 | Scanned / merged | Initializes optional nettrace start time to its explicit unset state before testing it. |
| !22649 | Scanned / merged | Fixes PROCMON error-path memory leaks. |
| !22648 | Deep / merged | Fixes CORBA IDL generated code that relied on unspecified C function-argument evaluation order. Together with !22671, strongly corroborates the later `c-expression-sequencing-conventions.md` rule. |
| !22647 | Scanned / merged | SMPP regression fix for issue #20891 caused by an earlier MR; targeted corrective change. |
| !22646 | Corroboration / merged | Avoids unsigned underflow when subtracting an FCS length from a too-short reported 802.11 frame; reinforces existing checked-arithmetic guidance. |
| !22645 | Scanned / merged | Parallelizes `check_static.py`, reporting roughly a 10x local speedup; tooling performance only. |
| !22644 | Corroboration / merged | Fixes RADIUS include-depth accounting by incrementing before testing the limit; consistent with existing recursion/depth-limit guidance. |
| !22643 | Scanned / merged | Earlier/focused nettrace proxy-source-port correction related to !22667. |
| !22642 | Deep / merged; promoted | Michael Mann states that `proto_tree_add_text_internal()` was never intended for dissectors because it creates unfilterable fields. The merged cleanup replaces uses with registered fields/subtrees or expert information. Promoted to `protocol-field-semantic-conventions.md`. |

## Durable notebook changes

- Added the explicit dissector rule against text-only unfilterable tree helpers to `protocol-field-semantic-conventions.md`, based on merged !22642.
- !22663, !22671/!22648, !22673, !22652, !22646, and !22644 were retained as corroboration of already stronger notebook guidance rather than duplicating rules.
- Closed draft !22689 was deliberately down-weighted.
