# Automated Wireshark MR review: !12512 through !12463

Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`

Notebook tracking consulted before selection: `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, and the available per-run ledgers under `reviewed-mrs-automation/`. The already-reviewed set was reconstructed from explicit MR numbers rather than treating numeric ranges as implicitly complete. The prior run ledger explicitly covers !12562 through !12513; no existing review tracking was found for the selected !12512 through !12463 entries. The historical !17571 through !17620 batch remains preserved and counted as previously reviewed.

## Exact reviewed set

Exactly these 50 MRs were reviewed in this run, in descending selection order:

!12512, !12511, !12510, !12509, !12508, !12507, !12506, !12505, !12504, !12503, !12502, !12501, !12500, !12499, !12498, !12497, !12496, !12495, !12494, !12493, !12492, !12491, !12490, !12489, !12488, !12487, !12486, !12485, !12484, !12483, !12482, !12481, !12480, !12479, !12478, !12477, !12476, !12475, !12474, !12473, !12472, !12471, !12470, !12469, !12468, !12467, !12466, !12465, !12464, !12463.

Batch outcome: 49 merged; one closed/unmerged draft (!12475). Master changes were treated as stronger implementation evidence than their stable-branch cherry-picks. !12475 had an empty change/commit set and conflicts and was therefore counted as reviewed but given essentially no positive implementation weight.

## Per-MR review notes

| MR | Outcome / depth | Review note |
|---|---|---|
| !12512 | Deep, merged master | R09 BCD fields switch from manual BCD extraction/string insertion to registered-field decoding via `proto_tree_add_item()` and encoding flags. Promoted with !12465. |
| !12511 | Scanned, merged master | Extends MDB communications-gateway expansion decoding with normal registered fields; no additional cross-cutting convention. |
| !12510 | Scanned, merged stable backport | MDB vending-message change; master behavior is stronger evidence than this cherry-pick. |
| !12509 | Scanned, merged master | Bluetooth ATT cleanup also corrects protocol-tree parentage for fields that had been attached below an unrelated Timestamp item. Local correctness fix, no new standalone rule. |
| !12508 | Deep, merged master | `asn2wrs` removes C-style comments during preparse. Martin Mathieson explicitly asked whether line numbering remains preserved; Anders Broman confirmed that preserving source lines was intentional and tested. Promoted to `source-preprocessing-conventions.md`. |
| !12507 | Corroboration, merged master | X.75 replaces manual packet-string copying with encoding-aware tvbuff string handling and fixes fuzzer warnings. Reinforces existing packet-derived string-safety guidance. |
| !12506 | Deep, merged master | John Thacker adds SSH channel-data reassembly and SFTP use through a TCP/TLS-style interface. Strong follow-on evidence for the corrected directional channel identity in !12503. |
| !12505 | Scanned, merged stable backport | `asn2wrs` accepts/ignores `WITH SUCCESSORS` where version semantics are not implemented. No additional broad convention. |
| !12504 | Corroboration, merged stable backport | BLF Ethernet interface identity uses channel plus hardware channel; master !12485 carries the stronger evidence. |
| !12503 | Deep, merged master | John Thacker fixes the assumption that both SSH endpoints use the same channel number. Pairings are learned from `SSH_MSG_CHANNEL_OPEN_CONFIRMATION`; per-peer namespaces and subdissector mappings are maintained. The MR also explicitly notes that channel-number reuse requires historical/generation-aware state for correct random access. Promoted to `stateful-reassembly-conventions.md`. |
| !12502 | Scanned, merged master | `fvalue` integer implementation consolidates on 64-bit internal storage while preserving narrower public getters. Internal simplification; no new rule. |
| !12501 | Scanned, merged master | `dftest -d` displays inferred types. Focused developer-tool enhancement. |
| !12500 | Scanned, merged stable backport | Qt Help-menu separator cleanup; no durable engineering rule. |
| !12499 | Scanned, merged stable backport | BLF Ethernet Status filtering fix; duplicate/backport evidence. |
| !12498 | Scanned, merged stable backport | Npcap 1.77 dependency update. |
| !12497 | Scanned, merged master | Npcap 1.75 -> 1.77 and checksum update; no substantive review discussion. |
| !12496 | Scanned, merged stable backport | SCTP dialog normalization; master !12482 is stronger evidence. |
| !12495 | Scanned, merged | Automated update; no reusable engineering convention extracted. |
| !12494 | Scanned, merged | Automated update; no reusable engineering convention extracted. |
| !12493 | Scanned, merged | Automated update; no reusable engineering convention extracted. |
| !12492 | Scanned, merged | Automated update; no reusable engineering convention extracted. |
| !12491 | Corroboration, merged stable backport | SCTP tap fixes `g_strlcpy()` use: destination-size is buffer capacity, not desired copy length. Master !12489 is stronger evidence. |
| !12490 | Scanned, merged master | Qt Help-menu separator cleanup; no durable engineering rule. |
| !12489 | Deep/corroboration, merged master | John Thacker fixes checksum-name truncation caused by passing `strlen(src)` as `g_strlcpy()` destination size. Strong API-contract example, but the notebook already has broader C API argument-contract guidance. |
| !12488 | Deep/corroboration, merged master | John Thacker enables/fixes SSH DH Group Exchange and keeps state-changing encryption/MAC setup on the first-pass path. Reinforces existing first-pass/stateful-dissection conventions. |
| !12487 | Scanned, merged master | `update-appdata.py` reports generated release entries; useful tool diagnostics, no new standalone rule. |
| !12486 | Scanned, merged stable branch | Source packaging uses `git stash create` to obtain a commit object without mutating the stash stack. Packaging-specific implementation evidence. |
| !12485 | Deep/corroboration, merged master | BLF Ethernet interface identity includes both channel and hardware channel for multiport interfaces. Reinforces modeling the complete interface identity. |
| !12484 | Scanned, merged master | BLF Ethernet Status filter fix; no new cross-cutting convention. |
| !12483 | Scanned, merged stable backport | Diameter S13/S13' User-Name IMSI decoding; protocol-specific. |
| !12482 | Discussion-focused, merged master | Jaap Keuter normalizes SCTP dialogs; John Thacker clarified the issue linkage in review. UI-specific, no broad rule promoted. |
| !12481 | Scanned, merged stable backport | MDB Optional Feature Enabled decoding; protocol-specific. |
| !12480 | Scanned, merged master | MDB communications-gateway config decoding; straightforward registered-field use. |
| !12479 | Scanned, merged stable backport | Source-tarball commit selection/stash logic; master packaging changes carry stronger evidence. |
| !12478 | Scanned, merged stable backport | Source packaging stash/version-info handling; duplicate/backport evidence. |
| !12477 | Scanned, merged master | Gerald Combs makes source-tarball generation include worktree version information via `git stash create`; packaging-specific. |
| !12476 | Scanned, merged stable backport | `update-appdata.py` update; duplicate/backport evidence. |
| !12475 | Down-weighted, closed/unmerged draft | Conflicted release-3.6 cherry-pick attempt with no changes or commits in the corpus snapshot. Counted as reviewed but not used as accepted implementation guidance. |
| !12474 | Scanned, merged stable backport | `update-appdata.py` update; duplicate/backport evidence. |
| !12473 | Scanned, merged stable backport | `update-appdata.py` update; duplicate/backport evidence. |
| !12472 | Scanned, merged master | Removes an unnecessary requirement that the most recent `wireshark-*` tag exist before generating appdata. Tool-specific. |
| !12471 | Scanned, merged stable backport | Freedesktop metainfo generation during tarball build; duplicate/backport evidence. |
| !12470 | Scanned, merged stable backport | Freedesktop metainfo generation during tarball build; duplicate/backport evidence. |
| !12469 | Scanned, merged master | Comment-only tooling correction. |
| !12468 | Scanned, merged stable backport | Freedesktop metainfo generation during tarball build; duplicate/backport evidence. |
| !12467 | Scanned, merged master | Gerald Combs updates Freedesktop metainfo as part of source-tarball packaging. Packaging-specific automation. |
| !12466 | Scanned, merged master | MDB communications-gateway setup-command dissection; protocol-specific. |
| !12465 | Deep, merged master | DECT NWK removes a local BCD conversion path and uses `proto_tree_add_item()` with `ENC_BCD_DIGITS_0_9` plus endian/odd-digit flags. Author noted compile-only validation due lack of a sample capture. Promoted with !12512 to `field-decoding-api-conventions.md`. |
| !12464 | Scanned, merged master | Trivial tvbuff indentation cleanup. |
| !12463 | Corroboration, merged master | John Thacker checks and propagates failure from `gcry_md_setkey()` instead of ignoring it. Reinforces checked-return/error-path conventions. |

## Notebook updates promoted from this batch

- `stateful-reassembly-conventions.md`: endpoint-local channel identifiers are directional aliases, not a shared stream ID; cross-peer pairings must be learned from protocol state, and reused identifiers need historical/generation-aware lookup for random access (!12503, reinforced by !12506).
- `source-preprocessing-conventions.md` (new): source-to-source preprocessing must preserve diagnostic source coordinates or maintain an explicit source map; test a diagnostic after removed/rewritten input (!12508, with explicit Martin Mathieson review).
- `field-decoding-api-conventions.md` (new): when registered field type plus `ENC_*` flags fully express the wire encoding, prefer standard protocol-tree decoding over manual extract/convert/add paths (!12465 and !12512).

The strongest authority in this batch came from John Thacker's authored-and-merged SSH state/reassembly work and Martin Mathieson's explicit source-location review on !12508. No substantive Guy Harris feedback surfaced in these selected 50 MRs; no Guy-derived rule was therefore inferred for this run.

Notebook commits created before this ledger:

- `311d45f0ba38aea7a6e38a94bfae6d560cbd8674` — document source preprocessing diagnostic stability.
- `b67029dd22223f46d59241a55e9d65bfd8dd02a9` — document directional channel identifier state.
- `cb3fb5b30bff34aaa8d2c0b3bf0f134fbc71ea10` — document declarative registered-field decoding.

## Frontier check

Frontier check only, not reviewed or counted in this run: !12462 (`mdb: show comms gw responses`) exists at corpus commit `ddcaa22b51c68f594e425a23388c3a2086813054` and is merged. The corpus therefore has additional previously unreviewed material below this batch and is not exhausted.