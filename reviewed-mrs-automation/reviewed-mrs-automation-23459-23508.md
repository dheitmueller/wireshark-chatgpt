# Wireshark MR Review Automation: !23459–!23508

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Before selecting this batch, the already-reviewed set was rebuilt from all available review tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md`, the aggregate automation tracking, and the individual ledgers under `reviewed-mrs-automation/`. Individual MR entries were used rather than assuming that a numeric interval was reviewed from a filename or neighboring batch. The historical !17571–!17620 batch remains explicitly preserved and counts as reviewed. No selected MR appeared in the existing tracking.

Exactly 50 MRs were reviewed, in descending order. The exact set is:

!23508, !23507, !23506, !23505, !23504, !23503, !23502, !23501, !23500, !23499,
!23498, !23497, !23496, !23495, !23494, !23493, !23492, !23491, !23490, !23489,
!23488, !23487, !23486, !23485, !23484, !23483, !23482, !23481, !23480, !23479,
!23478, !23477, !23476, !23475, !23474, !23473, !23472, !23471, !23470, !23469,
!23468, !23467, !23466, !23465, !23464, !23463, !23462, !23461, !23460, !23459.

## Review weighting and findings

Merged master work and substantive maintainer reasoning were weighted most heavily. Stable backports were used mainly as corroboration. Closed or superseded attempts were retained in the exact reviewed set but down-weighted except where their review discussion explains the merged successor.

- **!23507 with !23506/!23503 and !23504/!23505 — Deep/promoted.** Guy Harris traced stable-branch `validate-commit` failures to GitLab's generated merge subject, whose source-branch name pushed the subject beyond 80 characters even though the cherry-picked commit subject was compliant. The long-branch attempts were abandoned and merged successor !23507 used a shorter source branch. Promoted to `stable-branch-submission-conventions.md`.
- **!23493 with !23490 — Deep/promoted, merged master.** John Thacker corrected Bash-only `${var//.../...}` syntax introduced into Debian packaging rules by replacing it with a portable transformation suitable for the actual `/bin/sh` execution environment. Promoted to `portability-conventions.md`.
- **!23488 — Deep/promoted, merged master.** Authored and merged by Guy Harris: BLF writer interface mappings must expand when wiretap supplies a new IDB during dumping instead of assuming the interface set present at writer open is complete. Stable backport !23511 from the preceding batch corroborates this. Promoted to `wiretap-writer-conventions.md`.
- **!23460 with !23484 — Deep/promoted, merged master.** `check_spelling.py` relied on a module global initialized only in the main process, which happened to work with POSIX `fork` but failed under Windows multiprocessing `spawn`. The accepted fix gives worker-visible state a valid module-level definition; !23484 independently reinforces that developer checks are expected to run locally on Windows. Promoted to `python-tooling-conventions.md`.
- **!23502, !23500 and !23499 — Corroboration, merged.** BLF/JSON fixes reinforce explicit ownership cleanup and checking writer/tell failures. Existing ownership and writer-failure conventions already cover the durable principles.
- **!23498 — Discussion/tooling corroboration, merged.** Commit validation strips Git's `commit -v` scissors section so pasted diffs are not mistaken for commit-message body text. Useful evidence that validation should model the actual committed message, but not promoted separately.
- **!23495 — Discussion-focused, merged.** Pascal Quantin questioned provenance for externally maintained MCC/MNC operator data and asked about authoritative ITU sourcing. Useful reference-data review evidence, but the batch did not establish a sufficiently general new convention beyond existing standards/validation guidance.
- **!23494 — Corroboration, merged.** Removes unnecessary externally declared `value_string_ext` objects from a dissector header, reinforcing the existing rule to keep public/header surfaces limited to actual external consumers.
- **!23487 — Discussion-focused, merged first contribution.** Review caught an unsorted `value_string_ext`, which forces fallback from binary search, and the final MR used the proper dissector source location. Both points reinforce existing source-layout/value-table guidance rather than requiring new rules.
- **!23479, !23477, !23475 and !23474 — Corroboration, merged master/backports.** Pcapng custom-option writers distinguish logical option length from padding and reject values that cannot fit the 16-bit option-length field. Existing writer size/framing guidance covers the principle.
- **!23476 — High-authority review/corroboration, merged.** Guy Harris asked that an already-unnecessary first change be removed after !23488 solved it and that the MR be repurposed to the remaining FlexRay issue. This reinforces focused-MR and no-obsolete-workaround submission guidance.
- **!23473 — Corroboration, merged.** Broad dissector cleanup avoids fetching packet data twice where proto-tree return-value helpers already provide the decoded value. Existing fetch-once/helper-reuse guidance applies.
- **!23471, !23470 and !23464 — Corroboration, merged master/backports.** Fix an infinite loop by ensuring iterator progress on all paths and replace an allocated hash-value list with `GHashTableIter`, also removing a leak. Existing parser/control-flow and ownership conventions cover both principles.
- **!23469 and !23478 — Corroboration, merged.** Continue the unsigned TVB-search/offset migration and repair logic that previously depended on signed `-1` search sentinels. This is already extensively represented in parser/type-domain guidance from later batches.
- **!23481/!23480 — Corroboration, merged stable branches.** Zigbee Direct passes the required nested dissector context and composes columns around the subdissector call; existing context-flow and packet-column conventions cover this behavior.
- **!23459 — Scanned, merged.** NAS EPS fix recomputes the available length from the actual current offset and uses a remaining subset for nested dissection; consistent with existing TVB boundary guidance.
- **!23508, !23497, !23496/!23468, !23492, !23489/!23482/!23472, !23486/!23485, !23483, !23467/!23466/!23463, !23465, !23462 and !23461 — Scanned, merged.** Protocol-local, packaging-local, generated/reference-data, spelling-list, or straightforward cleanup changes that did not add a durable convention beyond notebook guidance already present.
- **!23491 — Down-weighted, closed.** Superseded stable-branch BLF backport attempt; the accepted behavior is represented by merged related work.

## Notebook changes

Promoted in this run:

- `stable-branch-submission-conventions.md`
- `portability-conventions.md`
- `wiretap-writer-conventions.md`
- `python-tooling-conventions.md`

Existing ownership, TVB/search, parser-progress, dissector context, packet-column, source-layout, public-header, writer-size, and helper-reuse conventions were intentionally not duplicated.