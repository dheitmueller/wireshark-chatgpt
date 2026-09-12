# Automated Wireshark MR review: !22342-!22391

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: reconstructed the already-reviewed set from the available per-run files in `reviewed-mrs-automation/` plus `reviewed-mrs.md`, preserving the historical !17571-!17620 batch. The 50 highest-numbered corpus MRs not already recorded as reviewed were then selected individually. For this run the resulting set happens to be the contiguous range !22391 through !22342; the range was not assumed reviewed or unreviewed as a unit.

Exactly 50 MRs were reviewed in this run, newest to oldest:

| MR | Review | Notes |
|---:|---|---|
| !22391 | Scanned | Yahoo Messenger: disable obsolete/niche dissector and heuristic by default; merged. Corroborates existing heuristic-default guidance. |
| !22390 | Scanned | ORAN FH CUS: make helper function static; merged maintenance cleanup. |
| !22389 | Scanned | Release-note cleanup; merged, no durable engineering convention. |
| !22388 | Scanned | Release-4.4 version/release preparation; no additional convention. |
| !22387 | Scanned | Release-4.6 version/release preparation; no additional convention. |
| !22386 | Scanned | Centralizes Npcap/USBPcap downloads through FetchArtifacts; merged build-infrastructure cleanup. |
| !22385 | Discussion-focused (closed) | Fortinet SSO heuristic-default proposal. Alexis La Goutte questioned disabling a commonly port-associated heuristic; Anders Broman explained the per-UDP-packet cost and Decode-As/opt-in fallback. Closed unmerged, so down-weighted. |
| !22384 | Scanned | E100 heuristic disabled by default; merged; same family as existing heuristic guidance. |
| !22383 | Scanned | DPlay heuristic disabled by default; merged; no additional rule beyond existing heuristic guidance. |
| !22382 | Scanned | Clique RM heuristic disabled by default; merged; no additional rule. |
| !22381 | Deep | John Thacker-authored/merged MS/TP TVBuff cleanup: protocol-reported PDU length remains the child TVB reported length; parent availability is for diagnostics/bounds handling rather than semantic clamping. Also prevents negative length on undersized vendor frames. Strong corroboration of existing TVBuff/bounds conventions. |
| !22380 | Scanned | CIGI heuristic disabled by default; merged; no additional rule. |
| !22379 | Scanned | Release build metadata for 4.4.11; no durable lesson. |
| !22378 | Scanned | Release build metadata for 4.6.1; no durable lesson. |
| !22377 | Scanned | AppleMIDI heuristic disabled by default; merged; no additional rule. |
| !22376 | Scanned | CAN-over-Ethernet heuristic disabled by default; merged; no additional rule. |
| !22375 | Scanned | BitTorrent heuristic disabled by default; merged; no additional rule. |
| !22374 | Scanned | Asphodel heuristic changed to default-off; merged; no additional rule. |
| !22373 | Scanned | NTLM SSP adds the documented LOCAL_CALL negotiate flag; straightforward protocol-definition update. |
| !22372 | Deep | John Thacker-authored/merged MS/TP CRC cleanup: reuse common CRC16-CCITT helper, verify against a known capture, and do not present encoded payload bytes as a checksum when the frame format does not carry that checksum. Corroborates helper reuse and capture-based regression validation. |
| !22371 | Scanned | AR.Drone heuristic-default and field/expert cleanup; merged; no additional general rule. |
| !22370 | Scanned | T.38 diagnostic heuristic made default-off because it is only relevant with a proprietary carrier protocol; corroborates existing heuristic guidance. |
| !22369 | Deep | Merged RFC 8654 BGP Extended Message support. Maintainers challenged preference-based capability handling; review established the partial-capture problem when the BGP OPEN exchange is absent. Added durable observed-negotiation/unknown-state fallback guidance to `derived-capability-conventions.md`. |
| !22368 | Scanned | ORAN FH CUS ST9 PRB/SINR display correction; merged protocol fix. |
| !22367 | Scanned | MS/TP source alignment cleanup; merged mechanical change. |
| !22366 | Scanned | Windows installer creates expected gcrypt configuration directory; merged packaging fix. |
| !22365 | Deep | John Thacker-authored/merged RTP TVBuff cleanup: use reported length directly, let TVBuff derive captured length, and do not attempt RTP desegmentation unless the entire reported payload is present. Strong corroboration of TVBuff and truncation semantics. |
| !22364 | Scanned | ISAKMP adds Fortinet/FortiClient EAP extension vendor ID; straightforward protocol update. |
| !22363 | Deep | John Thacker-authored merged IPSec-over-TCP cleanup removes manual caplen handling and a tree-dependent semantic path; payload dissection occurs before trailer access so truncated captures yield as much useful dissection as possible before bounds failure. Corroborates tree-independence and truncation guidance. |
| !22362 | Scanned | V5DL removes unnecessary `tvb_new_subset_length_caplen` uses, including disabled code; part of the same TVBuff cleanup series. |
| !22361 | Scanned | Transifex resource-name configuration update; no durable Wireshark coding rule. |
| !22360 | Scanned | Transifex resource-name configuration update; no additional rule. |
| !22359 | Scanned | Transifex resource-name configuration update; no additional rule. |
| !22358 | Discussion-focused (open snapshot) | SOME/IP optional dynamic service/method filter fields. Open in corpus snapshot; review feedback mostly local/style, so not treated as accepted architecture. |
| !22357 | Discussion-focused (open snapshot) | SOME/IP byte-array/UUID configuration. Michael Mann questioned continued proliferation of per-datatype UATs and suggested a standard externally parsed configuration format, but MR remained open with unresolved discussion; retained only as provisional architecture evidence. |
| !22356 | Deep | Merged SOME/IP container-filter work. Michael Mann explicitly requested separating whitespace-only changes because they obscured the functional diff; author revised accordingly. Strong corroboration of existing scope/reviewability guidance. |
| !22355 | Scanned | ATM pseudowire TVBuff conversion prevents dissection from leaking outside the protocol payload; merged, corroborating existing bounds guidance. |
| !22354 | Scanned | ORAN FH CUS validates reserved fields as zero; merged protocol validation improvement. |
| !22353 | Scanned (closed) | Transifex host/config update proposal; closed and down-weighted. |
| !22352 | Discussion-focused (closed) | Early disabled-protocol registration redesign. Jaap Keuter requested removing unrelated whitespace churn; MR closed unmerged after substantial discussion, so not treated as accepted architecture. |
| !22351 | Deep | Large merged RDP expansion/reassembly change with maintainer review. Martin Mathieson repeatedly preferred `val_to_str_const()` where no formatting substitution is needed, avoiding unnecessary copy/format scanning; useful helper-choice corroboration, but not promoted as a separate notebook rule. |
| !22350 | Deep | John Thacker-authored merged DOCSIS TVBuff/FCS fix: validates lengths before subtracting FCS, preserves Ethernet FCS semantics from protocol evidence, avoids premature exception on partially captured FCS, and relies on `proto_tree_add_checksum()` for its own bad-FCS expert item. Strong corroboration of checked-length and checksum semantics. |
| !22349 | Discussion-focused (closed draft) | Draft `-Wpointer-sign` enablement exposed a very large cleanup surface. John Thacker and Michael Mann discussed enabling the warning incrementally by component. Closed draft with failed pipeline; down-weighted versus later merged pointer-sign work already recorded in the notebook. |
| !22348 | Deep | John Thacker-authored merged conversion series removes manual `tvb_new_subset_length_caplen` uses where callers were reproducing `tvb_new_subset_length()` semantics. Strong corroboration of TVBuff API simplification. |
| !22347 | Scanned | DOCSIS specification/reference URLs updated to current public locations; documentation-only. |
| !22346 | Scanned | KDSP timeval represented as an actual time field rather than two unrelated integers; merged semantic field cleanup. |
| !22345 | Deep | John Thacker-authored/merged KDSP fix uses `FT_STRINGZTRUNC` for fixed-width storage containing a NUL-terminated string plus unspecified trailing bytes, and handles the defined NULL command without false expert warnings. Corroborates field types matching wire/storage semantics. |
| !22344 | Scanned | Release-4.4 preparation; no additional durable convention. |
| !22343 | Scanned | Release-4.6 preparation; no additional durable convention. |
| !22342 | Scanned | Documents `ENC_TIME_SECS_USECS` support in README.dissector; documentation alignment with an already-added API capability. |

## Durable notebook update

Added a section to `derived-capability-conventions.md`: negotiated capabilities should be learned from authoritative on-wire negotiation when available, but dissectors must distinguish an unknown negotiation state caused by a partial capture from an observed negative state. A narrowly scoped preference/fallback can be appropriate for that unknown case. Primary evidence: merged !22369 and its maintainer discussion.

The large heuristic-default-off cluster was not duplicated into the notebook because `heuristic-dissector-conventions.md` already contains the accepted rule that broad or costly heuristics should be opt-in when stronger protocol discriminators normally exist. Likewise, the TVBuff, tree-independence, pointer-sign, field-semantic, helper-reuse, and scope-discipline findings in this batch largely corroborate conventions already captured from later/stronger MRs.
