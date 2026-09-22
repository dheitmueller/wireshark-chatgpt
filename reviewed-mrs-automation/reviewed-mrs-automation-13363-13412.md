# Wireshark MR review automation ledger: !13363–!13412

## Corpus and selection

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Direction: descending from the newest available previously unreviewed MR.
- Selection method: rebuilt the already-reviewed set from the available `reviewed-mrs-automation/` per-run ledgers plus `reviewed-mrs.md` where applicable, using explicit MR numbers rather than assuming numeric ranges were complete. The historical !17571–!17620 batch remains counted as reviewed.
- The preceding run's lookup of !13412 was treated only as a frontier check, not as a prior review.
- Exact batch: the fifty MRs !13412 through !13363 inclusive.
- Status mix: 46 merged, 4 closed/unmerged (!13401, !13400, !13388, !13381).

## Exact reviewed set

| MR | State | Review weighting / note |
|---:|---|---|
| !13412 | merged | Reviewed |
| !13411 | merged | Reviewed |
| !13410 | merged | Reviewed; NULL column-append guard corroborates output/precondition discipline |
| !13409 | merged | Reviewed; NULL-safe `ws_label_strcpy` corroborates API precondition handling |
| !13408 | merged | Reviewed; stable backport of accepted display-filter grammar fix |
| !13407 | merged | Reviewed |
| !13406 | merged | Reviewed |
| !13405 | merged | Reviewed; stable backport |
| !13404 | merged | Reviewed; maintainer feedback removed unnecessary legacy-style `-1` initialization |
| !13403 | merged | Deep; John Thacker-authored bulk capture-capability retrieval; promoted |
| !13402 | merged | Reviewed; fixes Reset Layout regression caused by persisted splitter state |
| !13401 | closed | Down-weighted; direct translation edit superseded by Transifex workflow guidance |
| !13400 | closed | Down-weighted; direct translation edit superseded by Transifex workflow guidance |
| !13399 | merged | Deep; BLF forward-only/pipe-capable sequential reader; promoted |
| !13398 | merged | Reviewed; stable backport |
| !13397 | merged | Deep; only annotate actual TCP reassembly and expose completion frame; promoted |
| !13396 | merged | Reviewed |
| !13395 | merged | Reviewed; master-side TLS alert support |
| !13394 | merged | Reviewed; first-contributor TLS ECH alert update with draft references and manual tree validation |
| !13393 | merged | Reviewed; accepted display-filter identifier grammar fix |
| !13392 | merged | Reviewed |
| !13391 | merged | Reviewed |
| !13390 | merged | Reviewed |
| !13389 | merged | Reviewed |
| !13388 | closed | Down-weighted; empty/duplicate MR, with maintainer direction to keep discussion on the actual change |
| !13387 | merged | Reviewed |
| !13386 | merged | Reviewed |
| !13385 | merged | Reviewed; Debian shlibs/symbol-management policy evidence retained without overriding later packaging guidance |
| !13384 | merged | Reviewed; ARM64/Windows test-log severity adjustment |
| !13383 | merged | Reviewed; Qt geometry persistence improvement |
| !13382 | merged | Reviewed; fixes splitter-geometry replacement leak |
| !13381 | closed | Down-weighted; superseded early form of the display-filter grammar fix |
| !13380 | merged | Reviewed |
| !13379 | merged | Reviewed; later regression report linked to accepted !13402 follow-up |
| !13378 | merged | Reviewed; iSCSI LBA/binary-prefix fix with style review |
| !13377 | merged | Reviewed |
| !13376 | merged | Deep corroboration; explicit `pinfo->pool` / allocator-context refactor already represented in `semantic-context-parameter-conventions.md` |
| !13375 | merged | Reviewed |
| !13374 | merged | Reviewed |
| !13373 | merged | Reviewed |
| !13372 | merged | Reviewed |
| !13371 | merged | Deep; John Thacker-authored multi-interface JSON capability protocol; promoted with !13403 |
| !13370 | merged | Reviewed |
| !13369 | merged | Reviewed; stable backport |
| !13368 | merged | High-weight stable backport of Qt recursion fix |
| !13367 | merged | High-weight stable backport of Qt recursion fix |
| !13366 | merged | High-weight stable backport of Qt recursion fix |
| !13365 | merged | Deep; John Thacker-authored Qt signal-recursion correctness fix; promoted |
| !13364 | merged | Reviewed; lexer ambiguity/fixup evidence retained |
| !13363 | merged | Reviewed; master-side all-zero manufacturer special case |

## Durable findings promoted

### Batch privileged capability discovery, but keep per-interface outcomes independent

Merged master !13371 changes `dumpcap`'s machine-readable capability protocol to structured JSON that can return several interface results in one response. It attempts every requested interface and records per-interface errors/results instead of terminating the whole query on the first interface-specific failure. Merged master !13403 then changes the parent side to retrieve the complete interface capability set with one `dumpcap` invocation rather than one privileged helper launch per interface. The pair establishes a durable distinction between helper/protocol failure and an individual item's failure, while reducing startup cost and repeated Windows UAC prompts.

Promoted to `capture-discovery-cache-conventions.md`.

Notebook commit: `e64613d266cb376541fdc47737b6dd27f483e513`

### A sequential Wiretap reader should actually work on non-seekable input when the format is streamable

Merged master !13399 restructures the BLF reader around linear forward reading and explicitly enables pipe input. The durable lesson is to keep the sequential path forward-only and preserve state learned during the scan, while confining seek assumptions to random-access operations or formats that genuinely require them.

Promoted to `wiretap-read-cursor-conventions.md`.

Notebook commit: `94bff9cf2d345d9b784a2a647a96a0a5c96e1385`

### Reassembly presentation must be backed by positive framework state and should expose provenance

Merged master !13397 changes TCP's generic “segment of a reassembled PDU” annotation so it appears only when `reassembled_in` identifies an actual completion frame, and the annotation names that frame. A reassembly-capable code path is not itself evidence that reassembly happened.

Promoted to `reassembly-conventions.md`.

Notebook commit: `68a42bdda87dd999f8dae61ec561f6de16042976`

### Guard synchronous Qt signal feedback during programmatic model/view reconciliation

Merged master !13365 fixes a stack-smashing loop in Capture Options: `updateInterfaceColumns()` could call `setData()`, synchronously emit `dataChanged`/`itemChanged`, enter `interfaceItemChanged`, and call `updateInterfaceColumns()` again. The accepted fix suppresses the feedback signals while performing the internal reconciliation. The same correctness fix was carried to three supported release branches as !13366, !13367, and !13368, strengthening the evidence.

Promoted to new `qt-signal-recursion-conventions.md`.

Notebook commit: `d235b8116501a24fca80c339dd7bb48af709f52b`

## Corroborating and deliberately non-promoted evidence

- !13376 is strong historical corroboration for making packet-lifetime context explicit: reuse an existing `packet_info *`, pass `wmem_allocator_t *` to allocation-focused helpers, and use `pinfo->pool` in packet-oriented helpers. The notebook already contains the same durable rule in `semantic-context-parameter-conventions.md` from later accepted evidence, so this run did not duplicate it.
- !13393 plus backport !13408 reinforce context-sensitive lexer grammar: rules valid for a protocol name need not be identical to rules for later field-name components. !13364 similarly shows why a lexer fix must be checked against neighboring token classes such as `..`/DOTDOT. These were retained as parser evidence without creating another overlapping notebook rule.
- !13379 and !13402 form a useful regression/fix pair for persisted Qt layout state: restoring saved splitter geometry unintentionally changed the semantics of “Reset Layout,” and the follow-up explicitly clears the saved state before resetting.
- !13401 and !13400 were closed direct translation edits. Review discussion identifies Transifex as the translation source of truth. Because neither change merged, they are retained as workflow evidence but not elevated to a hard submission convention in this run.
- !13388 is an empty/duplicate closed MR. Guy Harris's response correctly asks what change was intended and points follow-up discussion back to the actual MR; useful review hygiene, but low implementation weight.
- !13385's Debian packaging change is retained as historical packaging-policy evidence. It was not promoted because later notebook material contains more directly applicable symbol/ABI packaging guidance and the exact policy is branch/distribution sensitive.
- !13363 and backport !13369 preserve Wireshark's pragmatic special treatment of all-zero manufacturer identifiers despite the formal IEEE assignment of OUI 00:00:00. This is important behavior but too domain-specific to elevate as a general architecture rule.

## Notebook commits made by this run

1. `e64613d266cb376541fdc47737b6dd27f483e513` — batch capture capability discovery and per-interface result semantics.
2. `94bff9cf2d345d9b784a2a647a96a0a5c96e1385` — non-seekable/forward-only Wiretap sequential-reader guidance.
3. `68a42bdda87dd999f8dae61ec561f6de16042976` — reassembly-state/provenance presentation guidance.
4. `d235b8116501a24fca80c339dd7bb48af709f52b` — Qt signal-recursion guard convention.

## Frontier check

After the review, the corpus still resolved to `ddcaa22b51c68f594e425a23388c3a2086813054`. MR !13362 exists and is merged. It was fetched only to confirm the next frontier and is **not** part of this reviewed set. Unless a newer corpus revision introduces higher-numbered unreviewed MRs, !13362 is the next descending candidate.