# Wireshark MR review: !13513–!13562

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`

Notebook starting commit: `ccef9f6cd91bfa9e9e1aa8639a0f75e93137a33c`

## Selection method

Before selecting this run, the reviewed set was rebuilt from the available tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md` and the per-run ledgers under `reviewed-mrs-automation/`. Only explicitly recorded MR numbers were counted; no numeric interval was assumed reviewed merely because neighboring MRs were present in a ledger. The historical reviewed batch !17571–!17620 was preserved and counted. The preceding run's lookup of !13562 was only a frontier check and did not count as review.

The fifty highest-numbered corpus MRs absent from that reviewed set were selected. Exactly fifty MRs were reviewed in this run.

## Exact MRs reviewed

!13562, !13561, !13560, !13559, !13558, !13557, !13556, !13555, !13554, !13553, !13552, !13551, !13550, !13549, !13548, !13547, !13546, !13545, !13544, !13543, !13542, !13541, !13540, !13539, !13538, !13537, !13536, !13535, !13534, !13533, !13532, !13531, !13530, !13529, !13528, !13527, !13526, !13525, !13524, !13523, !13522, !13521, !13520, !13519, !13518, !13517, !13516, !13515, !13514, !13513.

Status weighting: 44 merged; five closed/unmerged (!13562, !13549, !13541, !13540, !13535); one open/unmerged (!13560). Merged MRs were weighted more heavily, while closed, draft, superseded, and unresolved work was retained only as lower-confidence evidence where useful.

## Durable notebook findings promoted

### Section-scoped pcapng interface identity — !13559

Merged master MR !13559, authored and merged by John Thacker, fixes multi-section interface display and output by treating an interface reference as `(SHB/section, interface ID)` rather than assuming interface IDs are globally unique. It stores an explicit per-section-to-global mapping and uses that mapping when a multi-section input is flattened into the single-SHB form currently written by the dumper.

Promoted to `wiretap-interface-metadata-conventions.md` in notebook commit `7f175f71974554a3410eb82444fd9bf09e932c9a`.

### Address-copy helper must match destination lifetime — !13551

Merged master MR !13551, authored and merged by John Thacker, distinguishes temporary stack lookup keys from file-scope conversation state: the former use `copy_address_shallow()`, while persistent file-scope state uses `copy_address_wmem(wmem_file_scope(), ...)`. The durable rule is that an `address` copy is an ownership/lifetime decision, not merely a struct copy.

Recorded in new `address-copy-lifetime-conventions.md` in notebook commit `95757a391205f94412e538084f489d2992a4652b`.

### Checker invocation modes must cover the same semantic source domain — !13547

Merged master MR !13547, authored by Martin Kaiser and merged by Martin Mathieson, fixes `check_tfs.py` so whole-tree mode recursively reaches dissector subdirectories that commit-range mode could already reach. The durable rule is that whole-tree and changed-file checker modes should differ by filtering, not by silently using different definitions of which source files belong to the checker.

Promoted to `source-checker-conventions.md` in notebook commit `92a28f002660278b496f06a4cb5a8315d22c8b17`.

### Delimiter searches must stay inside the current grammar token — !13557/!13558

Merged master MR !13557, authored and merged by John Thacker, bounds SDP `fmtp` `=` lookup by `tokenlen` rather than searching the remainder of the tvbuff; accepted release-4.2 backport !13558 preserves the correction. The bug occurred when a token without `=` was followed by a later token containing one.

Promoted to `protocol-nesting-and-token-boundary-conventions.md` in notebook commit `7137b4c1eaf84c7575b3eafb6bf5d9af258a61f4`.

## Additional evidence retained without duplicating existing notebook rules

- !13526 is the merged master ZigBee TLV recursion-depth protection corresponding to stable backport !13552. It corroborates the existing parser-recursion guidance: externally controlled recursive structures require bounded dissection depth, balanced depth bookkeeping, and a diagnostic when the limit is reached.
- !13554's CIGI4 dissector review reinforces established new-dissector hygiene: remove unused fields/subtrees, avoid redundant field blurbs/initialization, keep abbreviations/prefixes consistent, and resolve duplicate registrations/checker findings before merge.
- !13539 plus merged follow-up !13555 reinforce supported-dependency compatibility testing: a Qt API use that built on newer Qt needed an explicit `QModelIndex()` argument to remain source-compatible with the project's older supported Qt version.
- !13548 and the closed !13535 provide lower-weight evidence that tests coupled directly to generated/registry-derived output can need adjustment when authoritative generated data changes; the closed MR was not promoted as a standalone convention.
- Closed !13541 contains high-authority Guy Harris submission feedback that an MR title should say what the change does and the description should provide the missing details, but because the MR did not merge this was treated as corroboration of existing submission guidance rather than a new hard rule.
- Open !13560 and closed !13562/!13549/!13540 were reviewed but down-weighted because their final upstream disposition did not establish accepted architecture.

## Frontier

The corpus was rechecked after review and remained at `ddcaa22b51c68f594e425a23388c3a2086813054`.

!13512 exists in that corpus and is merged. It was fetched only to verify the descending frontier and was **not** counted as reviewed. Absent newly scraped higher-numbered unreviewed material, !13512 is the next candidate for the following run.
