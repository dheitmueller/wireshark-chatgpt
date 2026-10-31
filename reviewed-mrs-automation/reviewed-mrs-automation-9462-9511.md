# Automated Wireshark MR review — !9511 through !9462

Review date: 2026-10-31
Corpus: `dheitmueller/wireshark-corpus-mrs`
**Corpus commit:** `ddcaa22b51c68f594e425a23388c3a2086813054`
Notebook starting commit: `b214814d0952747f5796b87b82db64b6e0c61640`
Direction: numerical descending, precise MR membership rather than numeric range inference.

**Exactly 50 distinct MRs reviewed:** 48 merged, 2 closed without merging (!9501 and !9468). All fifty `mr_N.json` corpus records exist and were inspected for MR metadata, discussion notes, and changed-file diffs. Not all giant generated diffs were available in full in the archive; no build, GUI testing, packet suite, or fuzz run was performed.

## Duplicate avoidance

Inventoried the **356 entries** in `reviewed-mrs-automation/`, consulted `reviewed-mrs.md` and the supplemental `reviewed-mrs-automation.md`, fetched the adjacent complete !9561–!9512 per-run ledger, and inspected all exceptional/noncontiguous/skip/plus/gap/exact-list tracking files for candidate overlap. All ordinary ledger filenames begin at !9512 or higher, and the aggregate/supplemental/special tracking showed no reviewed membership in !9462–!9511. The previous run's ledger documented explicit higher-numbered review coverage; its 50 unique entries were checked. The corpus tree was independently inspected and has all fifty candidate JSON records at the same commit. This membership check did **not** assume that an arbitrary numeric range is reviewed merely because a file covers part of it.

The historical `reviewed-mrs-automation-17571-17620.md` was independently fetched and counted: **50 unique reviewed MR table rows, all !17571–!17620 inclusive**. It remains unchanged.

## Exact reviewed MR set

| MR | Outcome | Depth | Evidence and assessment |
| --- | --- | --- | --- |
| !9511 | merged | Deep | João Valverde: RPM devel header installation explicitly covers Make, Ninja and CMake component paths. |
| !9510 | merged | Deep | João Valverde: Debian install-headers target/call restores SDK headers after default install changes. |
| !9509 | merged | Discussion-focused | Gerald and João distinguish public SDK files from unnecessary internal CMake helper modules. |
| !9508 | merged | Deep | Development CMake component excludes headers and metadata from ordinary runtime installs. |
| !9507 | merged | Scanned | Gerald release-3.6.11 notes with maintenance and security advisory updates. |
| !9506 | merged | Scanned | Gerald release-4.0.3 notes with security advisories and branch-specific fix IDs. |
| !9505 | merged | Scanned | John Thacker adds explicit QTime header for OpenSUSE Qt build compatibility. |
| !9504 | merged | Deep | Gerald AppImage launcher sets bundled library/data/extcap/plugin paths and documents safe overrides. |
| !9503 | merged | Scanned | release-4.0 backport of NSIS foundation donation page update. |
| !9502 | merged | Scanned | release-3.6 backport of same NSIS donation page update. |
| !9501 | closed | Discussion-focused | Alexis: stable branches backport bug fixes, not a PFCP grouped-IE refactoring enhancement. |
| !9500 | merged | Scanned | Master NSIS foundation donation link/text update, source for two stable backports. |
| !9499 | merged | Scanned | Logray desktop/MIME/AppStream integration and conditional install resources. |
| !9498 | merged | Scanned | Initial CPack binary package support for less-mainstream targets. |
| !9497 | merged | Deep | MinGW -Wclobbered fixes touch both ASN.1 sources and generated C; inspect uninitialized saved_offset possibility rather than copy blindly. |
| !9496 | merged | Scanned | Pascal NR RRC 3GPP v17.3 ASN.1 sources, template and generated output. |
| !9495 | merged | Discussion-focused | Alexis removes protobuf dead store; Anders questions unrelated .mailmap reordering in same MR. |
| !9494 | merged | Discussion-focused | Alexis fixes MySQL dead offset store; author disputes need to propagate return offset. |
| !9493 | merged | Scanned | BGP old-format filter abbrev renamed .old to avoid duplicate semantic key; description cites width warnings. |
| !9492 | merged | Scanned | release-3.6 backport of custom column occurrence-identity fix !9476. |
| !9491 | merged | Scanned | release-4.0 backport of custom column occurrence-identity fix !9476. |
| !9490 | merged | Deep | John documents FT_FRAMENUM strings as FRAMENUM_TYPE relationship metadata for packet list UI. |
| !9489 | merged | Deep | John replaces raw EtherCAT FoE column string copy with bounded encoding-aware tvb_get_string_enc. |
| !9488 | merged | Deep | John makes CredentialsModel destructor call clear() to release model-owned records. |
| !9487 | merged | Deep | John exempts FT_FRAMENUM strings from numeric val_to_str handling to prevent custom-column crash. |
| !9486 | merged | Deep | John gates DNS PTR-derived IPv4/IPv6 resolver updates to configured first-pass IN-class answers. |
| !9485 | merged | Scanned | Martin makes QUIC and HTTP2 local stream helpers static. |
| !9484 | merged | Scanned | Martin ORAN extension-11 bundle aligns to byte boundary, corrects unsigned PRB label format. |
| !9483 | merged | Scanned | Adds Nokia enterprise-specific PFCP IEs; main dissector diff not fully included in corpus. |
| !9482 | merged | Scanned | Pascal LTE RRC ASN.1 v17.3 source/template/generated update. |
| !9481 | merged | Scanned | release-3.6 backport of John iSCSI nested invalid-opcode Reject fix !9474. |
| !9480 | merged | Scanned | release-4.0 backport of John iSCSI nested invalid-opcode Reject fix !9474. |
| !9479 | merged | Scanned | Pascal LPP v17.3 ASN.1 source/template/generated update. |
| !9478 | merged | Scanned | João MSYS2 bcg729/libsmi/SBC dependency recipes and corresponding build documentation. |
| !9477 | merged | Deep | Martin fixes typed-item mask widths/duplicate abbreviations and documents RFC-based noncontiguous STUN exception. |
| !9476 | merged | Deep | Stig corrects custom-column duplicate identity to account for nonzero occurrence. |
| !9475 | merged | Deep | John rejects zero-byte Kafka compressed data before third-party codec calls, with expert note. |
| !9474 | merged | Deep | John handles unknown opcodes inside iSCSI Reject using fallback text and expert diagnostic. |
| !9473 | merged | Deep | João enables MSYS2 Lua5.1 and gates Windows Unicode-path tests on Lua Unicode patch feature. |
| !9472 | merged | Scanned | Fixes duplicate-word composite-TVBuff lifetime comment. |
| !9471 | merged | Scanned | Gerald automated master registry, translation and docs update; generated-data maintenance. |
| !9470 | merged | Scanned | release-4.0 automated registry/translation refresh, near-duplicate of !9471. |
| !9469 | merged | Scanned | release-3.6 automated registry/translation refresh, near-duplicate of !9471. |
| !9468 | closed | Discussion-focused | Rejected proposal to show only initial copyright year; commenter's legal assertions not treated as adopted policy. |
| !9467 | merged | Scanned | release-3.6 foundation link added to release notes. |
| !9466 | merged | Scanned | release-4.0 foundation link added to release notes. |
| !9465 | merged | Deep | ATSC3 ALC/LCT/LLS with sample capture; Alexis flags mask and Gerald post-merge Coverity nullable context. |
| !9464 | merged | Deep | João: MSYS2 UCRT64 support and intentional requirement to build inside supported MSYS2 shell. |
| !9463 | merged | Scanned | Gerald master foundation link added to release notes. |
| !9462 | merged | Deep | Gerald changes Qt ElidedLabel callers to plaintext and setSmallText after HTML-escaping behavior. |

## Durable conventions promoted

- Optional Development-component installs must be matched by Debian/RPM devel-package header installation and limited public SDK exports (!9508–!9511, João Valverde/Gerald Combs).
- AppImage relocation needs launcher defaults, runtime discovery and root/setuid restrictions checked together (!9504, Gerald Combs).
- `FT_FRAMENUM` `strings` is relationship metadata, not a normal `value_string` pointer; avoid crashing custom columns (!9490, !9487, John Thacker).
- Honor specific checker warnings while documenting spec-justified noncontiguous-mask exceptions (!9477, Martin Mathieson; !9493, Alexis La Goutte).
- Decode rejected malformed iSCSI headers with an expert marker, not a nullable opcode string (!9474/!9480/!9481, John Thacker). Guard zero-length Kafka codec input (!9475, John Thacker).
- Name-resolution data derived from PTR replies should be inserted only on the authoritative first pass and only when configured (!9486, John Thacker). Display wire text through `tvb_get_string_enc` (!9489, John Thacker).
- Column identity includes occurrence modifiers (!9476/!9491/!9492, Stig Bjørlykke); free model-owned records in the destructor (!9488, John Thacker).
- Detect optional Lua Unicode test capability rather than assuming it for Windows MSYS2 (!9473, João Valverde).
- ATSC3 dissector additions require nullable context audits: Gerald Combs identified Coverity null dereferences after !9465 merged; Alexis flagged a mask issue, and author referenced !12011. A merged status does not endorse those flaws.
- Closed !9501 gives explicit Alexis La Goutte stable-branch *bug fixes only* guidance, but its PFCP enhancement backport was not accepted. Closed !9468's copyright-year discussion is not normative legal guidance.
- Qt plaintext-only `ElidedLabel` callers must use widget styling APIs rather than embed markup (!9462, Gerald Combs).

## Evidence weighting and next run

Accepted master implementations and explicit expert feedback (John Thacker, João Valverde, Gerald Combs, Martin Mathieson, Alexis La Goutte, Stig Bjørlykke) were weighted above automated approval/merge chatter and close-only proposals. Stable cherry-picks corroborate master changes but are not independently counted as design precedents. The known post-merge Coverity findings and possibly uninitialized saved-offset pattern in !9497 are cautions, not promoted as recommended code.

**Next expected candidate: !9461**, subject to a fresh corpus inventory and exact reviewed-membership subtraction. The corpus is not exhausted; the recurring automation must remain enabled and unpaused.
