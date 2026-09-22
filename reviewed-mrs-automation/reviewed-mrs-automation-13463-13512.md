# Wireshark MR review: !13463–!13512

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`

Notebook starting commit: `a87dd517737f80309b78b4a2f0db61842e0bc5b0`

## Selection method

Before selecting this run, the reviewed set was reconciled from the available tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md`, the aggregate automation tracking, and the per-run files under `reviewed-mrs-automation/`. Only explicitly recorded MR numbers were counted; no numeric interval was assumed reviewed merely because neighboring MRs appeared in a ledger. The historical reviewed batch !17571–!17620 was preserved and counted. The preceding run's lookup of !13512 was only a frontier check and did not count as review.

The fifty highest-numbered corpus MRs absent from that reviewed set were selected. Exactly fifty MRs were reviewed in this run.

## Exact MRs reviewed

!13512, !13511, !13510, !13509, !13508, !13507, !13506, !13505, !13504, !13503, !13502, !13501, !13500, !13499, !13498, !13497, !13496, !13495, !13494, !13493, !13492, !13491, !13490, !13489, !13488, !13487, !13486, !13485, !13484, !13483, !13482, !13481, !13480, !13479, !13478, !13477, !13476, !13475, !13474, !13473, !13472, !13471, !13470, !13469, !13468, !13467, !13466, !13465, !13464, !13463.

Status weighting: 49 merged; one closed/unmerged (!13496). Merged master MRs were weighted most heavily; release-branch cherry-picks were used mainly as corroboration. !13496's corpus artifact is empty, so its state/discussion were recovered from the upstream GitLab record: it was Guy Harris's `DO NOT MERGE` code-signing MR, closed immediately because it targeted the wrong branch. It was therefore reviewed and recorded but intentionally given little architectural weight.

## Durable notebook findings promoted

### Preserve capture-option wire types and semantics — !13464, !13475, !13494

Three merged master MRs authored by Guy Harris form a coherent high-authority rule set. !13464 fixes `if_fcslen` to use its pcapng-defined unsigned one-byte type despite Wireshark's wider signed internal FCS representation; !13475 adds first-class signed integer Wiretap option types; and !13494 uses the signed `INT64` path for pcapng `if_tsoffset`, stores the per-interface offset, and applies it to packet timestamps. Guy also supplied a test pcapng and explicitly noted that the offset is signed in the specification.

Recorded in new `wiretap-option-type-conventions.md` in notebook commit `2e2695cc554af130306bfb111fd78cc4a7956964`.

### Cache keys must preserve resolver specificity — !13512

Merged master MR !13512, authored and merged by John Thacker, prevents 28/36-bit MA-M/MA-S manufacturer matches from being inserted into the 24-bit OUI cache. Reusing the coarser cache would cause unrelated addresses sharing the first 24 bits to inherit a more-specific vendor match.

Recorded in new `name-resolution-cache-conventions.md` in notebook commit `9aa91301df8a5001561ffd78c0230c343c0fef6d`.

### Keep result shape separate from failure signaling — !13491

Merged master MR !13491, authored and merged by John Thacker, makes capture capability discovery return the already-created empty capability hash table on recoverable failure rather than returning `NULL`. That both preserves one result-container contract and prevents leaking the table on error paths.

Recorded in new `api-result-container-conventions.md` in notebook commit `1c4715f76cd8ee354441e4289764fb60a15f11c4`.

## Additional evidence retained without duplicating existing notebook rules

- **!13511 — merged.** Review by Alexis La Goutte required network-type-specific address-length validation for Bitcoin `addrv2` and expert information for extra/missing data; the accepted revision also moved network dispatch to a `switch`. This reinforces existing malformed-length and protocol-discriminator validation guidance.
- **!13508 — merged.** DNS heuristic recognition was tightened by requiring a nonzero question/answer population and enough remaining bytes for the minimum possible encoded records. This strongly corroborates the existing heuristic-dissector rule that acceptance needs cheap structural invariants and negative/nonmatching traffic must be considered.
- **!13507 — merged.** SRT Key Material review plus pre-commit checking reinforce typed-field rules: Boolean/flag fields should use the appropriate encoding semantics, bit masks belong in the `hf_` registration, and a contributor need not invent a synthetic protocol-level grouping solely to use `proto_tree_add_bitmask`; a masked field can still be added directly with `proto_tree_add_item()`.
- **!13490 — merged.** Qt Resolved Addresses stopped concatenating structured values into one space-delimited string and then splitting it, because manufacturer names themselves can contain spaces. This corroborates the existing structured-serialization rule: preserve field boundaries structurally when payload strings may contain the delimiter.
- **!13489, !13485, !13484 — merged/backports.** Debian symbol-file maintenance reinforces the already-recorded ABI rule that exported symbol manifests must match actual public API names and newly exported functions exactly.
- **!13498/!13502 — merged master/backport.** TECMP heuristic detection remains compatible with older protocol versions by accepting a previously valid sentinel Device ID as well as the newer mandated value; useful protocol-version compatibility evidence, but too protocol-specific for a separate convention.
- **!13474/!13487 — merged master/backport.** IEEE 802.11 SAE parsing adds minimum-length and element-length checks to avoid false element detection and uses the same structural helper for confirm data; this corroborates existing bounded-parser/false-positive guidance.
- **!13480 — merged.** Coverity-driven ZigBee lifetime cleanup corroborates the existing ownership/error-path cleanup guidance.
- **!13496 — closed/unmerged, down-weighted.** The corpus file is empty; upstream GitLab shows Guy Harris immediately closed it as the wrong-branch backport and labeled it `DO NOT MERGE`. It establishes no accepted coding convention.

## Frontier

The corpus was rechecked after the review and remained at `ddcaa22b51c68f594e425a23388c3a2086813054`.

!13462 exists in that corpus and is merged. It was fetched only to verify the descending frontier and was **not** counted as reviewed. Absent newly scraped higher-numbered unreviewed material, !13462 is the next candidate for the following run.
