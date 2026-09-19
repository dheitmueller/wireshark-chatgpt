# Automated MR review ledger: !10100–!10149

Corpus commit reviewed: `08420a2f7851841b7a38714f642987c8cd167184`

Direction: descending from the newest available previously unreviewed MR toward older MRs.

## Exact reviewed set

This run reviewed exactly these 50 MRs:

`!10149, !10148, !10147, !10146, !10145, !10144, !10143, !10142, !10141, !10140, !10139, !10138, !10137, !10136, !10135, !10134, !10133, !10132, !10131, !10130, !10129, !10128, !10127, !10126, !10125, !10124, !10123, !10122, !10121, !10120, !10119, !10118, !10117, !10116, !10115, !10114, !10113, !10112, !10111, !10110, !10109, !10108, !10107, !10106, !10105, !10104, !10103, !10102, !10101, !10100`

The previously reviewed `!17571–!17620` batch remains part of the already-reviewed set. No numeric gap outside the exact ledgers should be inferred as reviewed.

## Review notes

The batch was scanned MR-by-MR from corpus metadata, discussions, and diffs, with merged work weighted above closed/abandoned work and maintainer-authored/approved changes weighted strongly.

### Durable findings

- **!10105 — Wiretap dump byte accounting (merged).** `wtap_dump_file_write()` now increments `wdh->bytes_dumped` itself, removing repeated manual accounting from many capture writers. Formats that seek back to rewrite headers save and restore the counter around the rewrite. This is durable API-design evidence: bookkeeping that is an invariant consequence of a primitive operation belongs in that primitive; exceptional non-linear operations should preserve/restore the invariant explicitly. Added to `api-design-conventions.md`.
- **!10128 — IPP UTF-8 truncation (merged, John Thacker; merged by Alexis La Goutte).** A bounded textual representation must not end in the middle of a multibyte UTF-8 character. The accepted fix uses `ws_utf8_truncate()` when the formatter reaches the end and asserts a minimally meaningful destination size. This corroborates existing string/UTF-8 safety guidance rather than requiring a new rule.
- **!10131 — ENRP recursion/NPE fix (merged, Gerald Combs).** Recursive dissectors must guard recursion and maintain valid packet context rather than assuming a non-recursive call topology. Useful corroboration for defensive dissector-state handling.
- **!10148 — UAT/Qt dissector-name editor (merged, John Thacker; LGTM from Stig Bjørlykke).** Validation/completion for machine-facing dissector identifiers should use the actual dissector-name syntax and must not accidentally impose lowercase-only policy. This reinforces the immediately preceding !10150 evidence about dissector names as machine-facing identifiers.
- **!10123 — Zebra heuristic recognition (merged, John Thacker; approved/merged by Alexis La Goutte).** When a protocol's conventional port is not formally registered, robust heuristic recognition is preferable to treating the port as authoritative. The implementation parses a candidate header and validates version/header structure before claiming the packet.
- **!10104 — MaxMind resolver fatal pipe errors (merged, John Thacker).** Worker-side pipe failures are converted into an explicit fatal response consumed by the main thread, which then tears down shared resolver resources and prevents future requests. This avoids synchronous lookup hangs during shutdown and reinforces explicit cross-thread failure signaling and centralized cleanup.

### Lower-weight / corroborative items

- **!10129** is a closed draft static-analyzer experiment and was not treated as accepted implementation precedent.
- **!10137** is closed and therefore weighted below the merged DoIP work in the same area.
- **!10147, !10146/!10145, !10138, !10122, and !10100** continue the project's pattern of fixing static-analysis, typed-item-checker, Valgrind, and uninitialized-state findings rather than ignoring them; existing notebook guidance already covers this.
- **!10139/!10136** consolidate rawshark around common dissection-option processing and shared documentation, reinforcing reuse of common option machinery instead of per-tool divergence.
- **!10110–!10115** are a related Wiretap encapsulation/accounting cleanup series; merged evidence emphasizes keeping per-file encapsulation semantics explicit and rejecting unsupported multiple-encapsulation behavior at the appropriate format boundary.
- **!10108/!10109** are release-CI/display-filter-list maintenance and do not establish a broader coding convention.

## Notebook changes

- Updated `api-design-conventions.md` with **Put operation-wide bookkeeping in the primitive that owns the operation**, based on merged !10105.
- No separate new convention was added for !10128, !10131, !10148, !10123, or !10104 because their durable lessons are already represented by existing string, dissector/state, identifier, heuristic, and lifecycle/cleanup guidance or are best retained here as corroborating evidence.

## Continuation

Rebuild the complete already-reviewed set from every ledger in `reviewed-mrs-automation/` plus `reviewed-mrs.md` before selecting the next batch. Subject to that reconstruction and the currently populated corpus, the next descending candidate is `!10099`.
