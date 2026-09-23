# Automated MR review: !12462 through !12413

Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Selection and tracking

Before selecting this run, the review set was reconstructed from the available per-run ledgers under `reviewed-mrs-automation/`, `reviewed-mrs.md`, and the supplemental automation tracker. The historical !17571-!17620 batch remains preserved and counted. The previous lookup of !12462 was treated only as a frontier probe, not as a completed review. No tracking entry marked any MR below !12463 as reviewed, so the fifty highest-numbered previously unreviewed MRs present in this corpus snapshot are !12462 through !12413 inclusive.

Exact reviewed MR numbers:

`!12462, !12461, !12460, !12459, !12458, !12457, !12456, !12455, !12454, !12453, !12452, !12451, !12450, !12449, !12448, !12447, !12446, !12445, !12444, !12443, !12442, !12441, !12440, !12439, !12438, !12437, !12436, !12435, !12434, !12433, !12432, !12431, !12430, !12429, !12428, !12427, !12426, !12425, !12424, !12423, !12422, !12421, !12420, !12419, !12418, !12417, !12416, !12415, !12414, !12413`

Batch outcome: all 50 corpus snapshots are merged. Master/original changes were weighted more heavily than release-branch copies and repetitive packaging/version updates.

## Review notes

| MR | Depth | Review result |
|---|---|---|
| !12462 | Scanned | Merged. MDB adds Communications Gateway response names and exposes the response in the Info column. Straightforward protocol presentation work. |
| !12461 | Scanned | Merged. IEEE 802.11 EHT multi-link field/filter names are reorganized, a typo is corrected, and missing reconfiguration capability fields are exposed. Useful naming consistency evidence but no new cross-cutting rule. |
| !12460 | Scanned | Merged. Fixes the release-note include path and corrects the AsciidoctorJ `--failure-level` comment. Build/documentation maintenance. |
| !12459 | Scanned | Merged. GitLab CI rules refinement; tooling-only evidence. |
| !12458 | Corroboration | Merged, authored and merged by Martin Mathieson. DCT2000 propagates SFN/slot information into MAC-NR packet context. Reinforces carrying known parent metadata into the downstream dissector; the code explicitly notes the slot interpretation limitation when SCS is unknown. |
| !12457 | Corroboration | Merged release-branch copy of !12450. SSH closes the ChaCha20 MAC context after use, reinforcing explicit ownership/cleanup. |
| !12456 | Scanned | Merged. Corrects source-package CI job rules. |
| !12455 | Scanned | Merged. Release-build metadata for 4.2.0rc1. |
| !12454 | Deep / corroboration | Merged master change authored and merged by John Thacker. SSH decryption is attempted only on the first pass; successful plaintext is cached for later passes and failed decryption is remembered without creating unnecessary file-scope state. Generated sequence-number items are marked generated. Strong corroboration for first-pass-only state mutation and deterministic redissection. |
| !12453 | Scanned | Merged. Starts MDB Communications Gateway support. Protocol-local extension. |
| !12452 | Scanned | Merged branch copy. Adds MDB Peripheral ID response dissection and reuses the shared ID helper. |
| !12451 | Scanned | Merged. Decodes Diameter User-Name as IMSI on S13/S13'. Protocol-specific presentation. |
| !12450 | Deep / corroboration | Merged master change authored and merged by John Thacker. Explicitly closes the per-message libgcrypt MAC handle after `gcry_mac_read()`, fixing a resource leak. Reinforces ownership cleanup at the point the resource becomes dead. |
| !12449 | Corroboration | Merged master change. Adds an IWbemLoginClientIDEx DCE/RPC dissector through PIDL/conformance inputs rather than hand-maintaining generated output. Reinforces generator-source ownership; no substantive review discussion added a new rule. |
| !12448 | Scanned | Merged. MAC-NR adds the UL CE Timing Advance Report. Protocol-local feature. |
| !12447 | Scanned | Merged branch copy of the MDB Peripheral ID response work. |
| !12446 | Scanned | Merged. Adds MDB vending-message dissection and widens header fields in preparation for Expanded Currency Mode. Protocol-local evolution. |
| !12445 | Scanned | Merged. Adds MDB Expansion-message scaffolding and moves repeated ID parsing into a helper. |
| !12444 | Corroboration | Merged branch copy of !12441. Carries the SSH malformed-length/bounds/progress fixes to a maintained branch. |
| !12443 | Corroboration | Merged branch copy of !12441. Same accepted SSH safety fix on another maintained branch. |
| !12442 | Corroboration | Merged branch copy of !12440. Prevents duplicate closing of SSH cipher state. |
| !12441 | Deep / corroboration | Merged master change authored and merged by John Thacker. Bounds raw SSH ciphertext access to the extent actually requested from the TVBuff, validates malformed block lengths, removes inappropriate CBC ciphertext-stealing mode, and returns a terminal captured-length result on failure instead of an unchanged offset that can cause an infinite loop. Strongly reinforces existing TVBuff-bounds and parser-progress rules. |
| !12440 | Deep / corroboration | Merged master SSH lifetime fix. Ensures cipher handles are not closed both on an error path and again by file-scope destruction. Reinforces single-owner destruction. |
| !12439 | Scanned | Merged. Version bump 3.6.18 to 3.6.19. |
| !12438 | Scanned | Merged. Version bump 4.0.10 to 4.0.11. |
| !12437 | Corroboration | Merged branch variant of the macOS libbcg729 install-name/RPATH workaround. |
| !12436 | Scanned | Merged. Build metadata for 4.0.10. |
| !12435 | Deep / corroboration | Merged master first-contribution RF4CE dissector. Review explicitly asks for a shareable capture, fuzzing, release-note integration, and resolution of dissector-check warnings. Gerald Combs supplies the ASan/fuzz-test invocation. This strongly corroborates existing new-dissector submission and fuzz-validation guidance. |
| !12434 | Corroboration | Merged branch variant of the macOS libbcg729 workaround. |
| !12433 | Deep / corroboration | Merged release-branch libbcg729 workaround, authored/merged by Gerald Combs. Guy Harris explains the underlying CMake `@rpath` install-name behavior and notes that the newer `macos-setup.sh` CMake invocation should eliminate the problematic dependency layout once libraries are rebuilt. High-authority platform-build evidence, but it fits existing deployment/runtime-capability guidance rather than requiring a duplicate rule. |
| !12432 | Scanned | Merged. Version bump 3.6.17 to 3.6.18. |
| !12431 | Scanned | Merged. Version bump 4.0.9 to 4.0.10. |
| !12430 | Scanned | Merged. Build metadata for 3.6.17. |
| !12429 | Scanned | Merged. Build metadata for 4.0.9. |
| !12428 | Scanned | Merged. Release preparation for 3.6.17. |
| !12427 | Scanned | Merged. Release preparation for 4.0.9. |
| !12426 | Scanned | Merged branch CI update from EOL OpenSUSE 15.3 to 15.4. |
| !12425 | Scanned | Merged branch copy of the Windows packaging XML tag fix. |
| !12424 | Scanned | Merged master Windows packaging fix closing a missing XML component tag. |
| !12423 | Scanned | Merged. MDB Optional Feature Enabled dissection. |
| !12422 | Corroboration | Merged master change authored/merged by Gerald Combs. Aligns the Intel macOS package CI deployment target with macOS 11.0; reinforces explicit deployment-target consistency. |
| !12421 | Deep / promoted | Merged master correctness fix reviewed and merged by John Thacker. A pcapng Section Header Block's byte-order magic must be established before interpreting its block length; otherwise a later section with different endianness can claim the wrong byte extent. John's review specifically favors handing invalid-magic input to the normal parser through a safely bounded 12-byte minimum subset instead of duplicating parsing/error logic. Promoted to `framing-boundary-conventions.md`. |
| !12420 | Scanned | Merged master CI optimization by Gerald Combs. The Windows Qt5 build runs automatically for upstream MR UI changes and remains a manual optional job otherwise. |
| !12419 | Scanned | Merged spelling fixes. |
| !12418 | Deep / promoted | Merged master change authored and merged by John Thacker. `ws_regex_matches_pos()` now accepts a subject offset and passes it to PCRE2 while preserving the full subject; shortening the subject is not semantically equivalent when lookbehind or other context-sensitive assertions are involved. Promoted to `search-offset-conventions.md`. |
| !12417 | Scanned | Merged. Second CMake correction for release-note dependency ordering. |
| !12416 | Deep / promoted | Merged master change authored and merged by John Thacker. Packet Find stores the match start rather than the last matched byte and treats length separately, eliminating confusing endpoint arithmetic and making zero-length regex matches representable. Promoted to `search-offset-conventions.md`. |
| !12415 | Scanned | Merged. `asn2wrs` accepts/ignores the ASN.1 `WITH SUCCESSORS` directive because module-version checking does not make it semantically useful here. |
| !12414 | Deep / corroboration | Merged master HTTP heuristic hardening. John Thacker catches an important contract bug in the initial proposal: `-1` already represents HTTP needing desegmentation and cannot also mean definite protocol rejection. The accepted version uses a distinct `-2` internal result for failed optional ASCII validation and maps only that to dissector rejection. Strong corroboration for the existing TCP desegmentation/ownership rule. |
| !12413 | Scanned | Merged. CMake release-note dependency correction; build-graph maintenance. |

## Durable notebook updates

- `framing-boundary-conventions.md`: added the rule that representation-defining framing discriminators (notably byte-order magic) must be established before dependent lengths/offsets are interpreted; malformed discriminator paths should use a safe bounded view rather than guess an interpretation. Evidence: !12421, including John Thacker's accepted review direction. Commit: `3f3da0540b531c67b1ca52d4efd9a289e1d0dcf5`.
- `search-offset-conventions.md`: new file recording start-plus-length range representation and the requirement to preserve a full regex subject while using the engine's start-offset parameter. Evidence: !12416 and !12418, both authored and merged by John Thacker. Commit: `44b36b39c7c1c78fa5973f117838059c1d3961cb`.
- !12414, !12454, !12450, !12441, !12440, !12435, !12433 and related backports were retained as corroborating evidence for existing desegmentation, first-pass/redissection, ownership, TVBuff-boundary, submission/fuzzing, and platform-build conventions rather than duplicating already-recorded rules.

## Frontier check

!12412 (`YACC: Update yacc.py to 3.11`) exists in this same corpus snapshot and is merged. It was inspected only to establish the next frontier and is **not** counted as reviewed in this run. The corpus therefore has not run out of unreviewed MRs.