# Automated Wireshark MR review: !12763-!12812

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Review direction: descending from the newest available previously-unreviewed MR.

Selection was rebuilt from the current explicit review tracking in `reviewed-mrs.md`, the supplemental automation tracker, and the available per-run ledgers under `reviewed-mrs-automation/`. The immediately preceding ledger marks !12812 only as a frontier probe, not a review. Searches of the current notebook found no earlier review record for the remaining candidates in this batch, and the historical !17571-!17620 ledger was independently verified and remains counted. The notebook was at the exact HEAD produced by the preceding completed run before this batch began.

This run reviews exactly these 50 MRs:

`!12812, !12811, !12810, !12809, !12808, !12807, !12806, !12805, !12804, !12803, !12802, !12801, !12800, !12799, !12798, !12797, !12796, !12795, !12794, !12793, !12792, !12791, !12790, !12789, !12788, !12787, !12786, !12785, !12784, !12783, !12782, !12781, !12780, !12779, !12778, !12777, !12776, !12775, !12774, !12773, !12772, !12771, !12770, !12769, !12768, !12767, !12766, !12765, !12764, !12763`

Status weighting: 48 records are merged. !12810 and !12795 are closed/unmerged and are deliberately lower-weight than accepted work. !12810 explicitly says it was replaced by !12844, which was itself already reviewed as a closed draft in the previous batch; the merged !12840/!12861 work remains stronger evidence for IPv4 representation conventions. !12795 is an aborted release-build attempt superseded immediately by merged !12796.

## Durable findings

- **!12812 — deep, merged master, very high weight.** John Thacker's OER fix makes a generic integer parser honor the registered field's signedness. The helper can target signed or unsigned integer fields; the accepted implementation queries the field type, sign-extends short negative wire encodings before accumulation, and uses the matching `proto_tree_add_int()` or `proto_tree_add_uint()` API. Unexpected field types remain programmer-invariant failures instead of being silently coerced. Added `numeric-field-signedness-conventions.md`.

- **!12769 — deep, merged master, extremely high weight.** `mergecap` could enter an infinite feedback loop if append mode wrote to a file that was also one of its inputs. The accepted fix checks the output against every input using `files_identical()` before entering the merge operation and reports a specific invalid-option error. Guy Harris committed and approved the final change, so this is especially authoritative evidence that file safety checks must compare filesystem identity, not raw path spelling, and should reject unsafe input/output aliasing before the write path begins. Added `file-identity-safety-conventions.md`.

- **!12768 + !12771 — deep/corroborating merged master/backport pair.** ED137 called its additional-feature subdissector only when a protocol tree was being built. That meant a tree-less first pass could not use `visited` correctly or build conversation/request-response state needed later. This independently reinforces the existing `stateful-reassembly-conventions.md` rule that `proto_tree` availability is presentation state and must not gate persistent protocol analysis. No duplicate convention was added.

- **!12775 + !12781 — substantive merged master/backport pair.** RTPS string handling now copies fixed-buffer topic/type names and then uses `ws_utf8_truncate()` so truncation never ends in the middle of a UTF-8 code point. The same change makes the encoded string-length field explicit in the tree and replaces manual `tvb_get_string_enc()` plus `proto_tree_add_string()` with direct protocol-tree APIs. This strongly corroborates the existing `string-api-conventions.md` UTF-8 truncation guidance, so no duplicate rule was added.

- **!12794 — deep, merged master.** The ED137 request/response enhancement came with a deliberately forged focused capture covering pure RTP, header-extension variants, multiple SQI methods/values including an expert-info error case, CLIMAX forms, radio remote control, RMM/MAM combinations including missing response, and ED137B/C NMR forms. Human review then corrected small representation/format issues. This is strong additional evidence for the notebook's existing practice of supplying compact captures that cover both changed behavior and edge/error cases rather than only a happy-path packet.

- **!12784 — deep, merged master, first contribution.** Anders Broman's review of the new AUTOSAR BusMirroring dissector requested the project-standard SPDX identifier, sanity checking/using the protocol DataLength, clearer names, and the normal incremental-offset parsing style. The author revised accordingly and the dissector merged. These points corroborate existing dissector length-validation, licensing/style, and sequential-offset conventions rather than creating a distinct new rule.

- **!12782 + !12786 + !12787 — substantive merged master/backport family.** IEEE 1722 was passing a partially initialized `can_info` structure into SocketCAN subdissectors. The accepted master fix fills the decoded payload length, CAN-FD flag, and bus identifier before the nested call, then was backported to both supported release branches. This corroborates the existing caller-to-subdissector context contract: caller-supplied metadata must be complete before dispatch.

- **!12798 + !12807 — merged master/backport.** Diameter registry work received concrete review on semantic names and newly occupied bits; Pascal Quantin requested renaming a field after bits 8 and 9 became defined, and another reviewer caught missing/incorrect AVP names. The author revised the data before merge. Useful evidence that registry/spec updates require auditing nearby names and masks, not only appending new constants, but too protocol-specific for another general rule.

- **!12789 + !12766 — merged master, John Thacker-authored.** Follow Stream state is now reset when a retap/filter change interrupts a tap, preventing old half-completed payload from being duplicated, and the Qt dialog uses the shared `follow_reset_stream()` after fixing that common cleanup path instead of maintaining its own variant. These changes reinforce state-reset completeness and common-cleanup ownership rather than adding a new convention.

- **!12810 — closed/unmerged and superseded; deliberately down-weighted.** João Valverde proposed explicit host-order/network-order IPv4 typedefs because the old generic type made representation difficult to reason about, then closed it with the note that it was replaced by !12844. It corroborates the already-recorded `address-representation-conventions.md` motivation, but the merged !12840/!12861 fixes remain the stronger implementation evidence.

- **!12795 — closed/unmerged release-build attempt; deliberately down-weighted.** It was followed immediately by merged !12796 for the same 4.2.0rc3 build. No durable lesson is taken from the abandoned instance.

## Additional reviewed material

The remaining MRs were individually inspected and counted. They include release backports; Python 3.12/CMake cleanup (!12808/!12809); TLS registry expansion (!12806); GLib updates (!12803/!12805); ISOBUS master/backport work (!12797/!12804); PowerShell CI hashing (!12801/!12802); multi-type plugin descriptions (!12800); release-note/build plumbing (!12799/!12796); Qt Follow Stream keyboard/focus fixes (!12792/!12793); IANA IP registry generation/search/fixups (!12770/!12778/!12785/!12791); Bitcoin value-table updates (!12790); display-filter function/plugin refactors (!12772/!12773/!12776/!12777/!12788); Qt/Clang compatibility (!12783); Falco plugin cleanup (!12779); Korean translation and Transifex API maintenance (!12774/!12763/!12764); `std::move`/dead-state cleanup in Follow Stream (!12780/!12765/!12767); and the RDP EGFX crash/negotiation fix (!12811). None supplied stronger new general guidance than the conventions above or existing notebook material.

## Notebook updates

- Added `numeric-field-signedness-conventions.md` from merged !12812.
- Added `file-identity-safety-conventions.md` from merged !12769, with Guy Harris's commit/approval given high authority.
- Retained !12768/!12771 as corroboration of `stateful-reassembly-conventions.md` and !12775/!12781 as corroboration of `string-api-conventions.md` rather than duplicating already-established rules.
- Retained closed !12810 only as lower-weight corroboration of `address-representation-conventions.md`.

## Frontier

`!12762` exists in the corpus (`Translation: Update to Transifex API v3`) and is merged. It was inspected only as the next descending frontier candidate and is **not** counted as reviewed by this ledger. The corpus is therefore not exhausted.
