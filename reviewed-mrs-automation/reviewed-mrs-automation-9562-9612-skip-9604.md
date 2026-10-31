# Automated Wireshark MR review — !9612 through !9562, excluding !9604

Review date: 2026-10-31
Corpus repository: `dheitmueller/wireshark-corpus-mrs`
**Corpus commit:** `ddcaa22b51c68f594e425a23388c3a2086813054`
Notebook starting commit: `97ecdf223e944feb7baa023d8038ba6bc4b455e3`
Direction: descending numerical MR number, explicitly excluding prior reviewed membership.
**Exactly 50 distinct actual MR records: 48 merged and 2 closed without merge (!9603 and !9600).**

## Selection and historical tracking verification

Read `reviewed-mrs.md`, the supplemental `reviewed-mrs-automation/reviewed-mrs-automation.md`, and all 354 per-run/exceptional files in `reviewed-mrs-automation/`, including 337 conventional-range files and 17 exceptional/supplemental records. Independently checked explicit membership descriptions and the exceptional/noncontiguous batches, instead of treating named intervals as complete. The 60 higher-numbered records missing from ordinary filename ranges were all explicitly accounted for by exceptional tracking or individual entries in `reviewed-mrs.md`. The `reviewed-mrs-automation-17571-17620.md` historical batch was fetched and its **50 distinct table entries** independently counted; it is preserved unmodified.

The latest prior run explicitly reviewed !9662–!9613. The fifty highest-numbered remaining records in the unchanged corpus are precisely !9612 down to !9562, **omitting !9604**, whose `mr_9604.json` does not exist in this corpus commit. This is a *noncontiguous* exact-membership batch, not a claim that every numeric ID exists. All fifty existing JSON records were fetched and examined for status, non-system discussion, and available `changes_data`; significant MRs received deeper diff/comment analysis. !9603's large main dissector patch is omitted in the archive, so no unsupported implementation inference is drawn from it. This review did not execute compilation, dissector tests, or fuzzing.

## Exact reviewed MRs

| MR | Outcome | Review depth | Evidence and finding |
|---|---|---|---|
| !9612 | merged | Scanned | Guy Harris TWAMP MBZ 16-bit field correction; release backport of the same change as !9592. |
| !9611 | merged | Scanned | Companion TWAMP MBZ FT_UINT16 backport, not independent policy evidence. |
| !9610 | merged | Deep | João Valverde Linux RPATH and static dumpcap wsutil attempt; macOS linked static and dynamic wsutil and aborted; later reverted in !9647, stronger successor !9793. |
| !9609 | merged | Scanned | John Thacker BGP EVPN ADD-PATH recognition in NLRI parsing. |
| !9608 | merged | Deep | John Thacker preserves growing pcapng NRBs in editcap, tshark, mergecap with per-input and dumper cursors; avoids duplicate resolved-address output; remaining TShark one-pass limitations noted. |
| !9607 | merged | Deep | João Valverde derives relocated Unix data/plugin/extcap paths from executable; Guy Harris questioned platform layouts and Gerald found macOS build-directory plugin failure corrected in !9621. |
| !9606 | merged | Scanned | HTTP/2 stream request and response frame references for cross-navigation. |
| !9605 | merged | Scanned | Fix inconsistent SAPHDB/SAPRouter hf and expert field types; warning-driven correction. |
| !9603 | closed | Discussion-focused | Unmerged Delivery Optimization dissector; large implementation patch unavailable in corpus, Chuck Craft asked about unpublished blog documentation. |
| !9602 | merged | Scanned | Correct text and AWDL display-filter abbreviation typo. |
| !9601 | merged | Discussion-focused | RTPS header extension and protection; Jaap Keuter requested enum ordering and defines, Alexis La Goutte flagged dead offset store. |
| !9600 | closed | Discussion-focused | Direct German .ts translation patch closed; Alexis La Goutte directed corrections through Transifex. |
| !9599 | merged | Scanned | Guy Harris backport normalizes file extensions to lowercase for case-insensitive opener matching. |
| !9598 | merged | Scanned | Second Guy Harris extension-normalization backport; duplicate code evidence. |
| !9597 | merged | Deep | Pascal Quantin: exported-PDU TCP tag fields should be namespaced, decoded with typed ret APIs, and nonzero Boolean normalized to true; unknown versions use expert info. |
| !9596 | merged | Deep | Wiretap appendable Exported-PDU TLV builders: clean per-record buffers, append at end, advance valid length; post-merge MSVC size_t-to-guint16 warnings fixed separately in !10048. |
| !9595 | merged | Discussion-focused | GREBonding display filters need meaningful lowercase names and no trailing whitespace, per Alexis La Goutte. |
| !9594 | merged | Discussion-focused | Pascal Quantin reviewed extension-case comparisons; accepted path normalizes ASCII once using ascii_strdown_inplace rather than repeated allocation. |
| !9593 | merged | Scanned | Make typed-item checker regex tolerate whitespace between function identifier and opening parenthesis. |
| !9592 | merged | Discussion-focused | TWAMP two-byte MBZ fields require FT_UINT16; Martin Mathieson diagnosed checker miss caused by whitespace before parenthesis, addressed in !9593. |
| !9591 | merged | Scanned | UDS error service decoder includes OBD service identifiers. |
| !9590 | merged | Scanned | UDS negative response code is NRC, not service ID; wording correction. |
| !9589 | merged | Scanned | O-RAN CUS codepoint descriptions and compression-header subtree. |
| !9588 | merged | Scanned | SMB2 field label corrected from number to index. |
| !9587 | merged | Scanned | BTLE opcode value_string copy/paste duplicate key corrected. |
| !9586 | merged | Discussion-focused | MySQL dissection without observed capability handshake adds heuristic; Alexis La Goutte suggested configurable fallback preference, not implemented here. |
| !9585 | merged | Scanned | Reduce misleading MySQL length-encoded integer tree output. |
| !9584 | merged | Scanned | Routine automatic vendor IDs, translations, and data refresh. |
| !9583 | merged | Deep | SAPRFC built-in dissector; Alexis/Martin caught repeated hf abbreviation, questionable dissector table choice, unknown-value formatting, and checker warnings. |
| !9582 | merged | Scanned | SAPRouter credentials tap integration accompanied by a small sample capture. |
| !9581 | merged | Deep | SAPDiag built-in dissector; Martin Mathieson and Alexis caught FT width vs tvb length mismatch, dead stores, incompatible duplicate hf registrations, spelling warnings. |
| !9580 | merged | Scanned | Correct release-note personal extcap destination path. |
| !9579 | merged | Deep | MySQL parsing advances offset regardless of optional proto_tree, preventing inconsistent first-pass/redissection consumed lengths. |
| !9578 | merged | Scanned | Routine automatic release-3.6 generated files/data update. |
| !9577 | merged | Scanned | Routine automatic release-4.0 generated files/data update. |
| !9576 | merged | Scanned | IEEE-1905 EasyMesh capability bits; short commit lines and correct whitespace enforced. |
| !9575 | merged | Scanned | OpenSUSE GitLab CI image migrated from EOL 15.3 to 15.4. |
| !9574 | merged | Discussion-focused | John Thacker updates RPM toolchain requirements, Qt default by distro, CMake and library dependencies. |
| !9573 | merged | Deep | John Thacker preconsumes pcapng initial internal blocks (SHB IDB NRB DSB etc.) before first packet; plugin custom blocks stay on regular caller path; custom workaround still needed. |
| !9572 | merged | Deep | John Thacker adapts unittest result collection to Python 3.11 _outcome.result while handling prior versions; pytest caveat remains. |
| !9571 | merged | Scanned | Dev headers installed only with explicit CMake Development component; release note. |
| !9570 | merged | Discussion-focused | WPS flag masks, trailing whitespace, and short commit-line reviews by Alexis La Goutte. |
| !9569 | merged | Discussion-focused | RPM openSUSE install fix; John Thacker advised distro mock builds and replacing proliferation of version tests, later !9574. |
| !9568 | merged | Scanned | Debian install-headers build-tool selection for Ninja vs Make. |
| !9567 | merged | Scanned | Use Qt-version guard for pre-5.14 QString split enum compatibility. |
| !9566 | merged | Scanned | Personal extcap directory migration must update actual discovery path, not just documentation. |
| !9565 | merged | Scanned | RPM %docdir classifies documentation; package must separately list installed directory. |
| !9564 | merged | Discussion-focused | MySQL caching_sha2_password implementation; Martin Mathieson requested accurate unused annotations, avoiding redundant init/unused code. |
| !9563 | merged | Discussion-focused | Logray does not support Follow Stream; John Thacker caught unrelated commented-out code in UI removal patch. |
| !9562 | merged | Deep | Martin Mathieson corrects typed-item ENC and width calls across 15 dissectors and widens checker detection. |

## Durable notebook decisions

- **!9608, !9573 (accepted master; John Thacker):** preserve growing pcapng NRBs alongside DSBs and IDBs and drain after the last packet; consume initial known internal blocks even before the first packet; leave plugin-defined custom block semantics to their registered reader path. Be explicit about the still-incomplete TShark-generated NRB behavior in this historical version.
- **!9596, !9597:** keep generated Exported PDU tag builders append-safe, with per-record reset and correct logical buffer length; parse TCP dissector metadata using typed return APIs, actual nonzero Boolean semantics, and expert warnings for unknown versions. Pascal Quantin's review and John Thacker's MSVC portability reports were weighted highly.
- **!9610, !9607:** Guy Harris's loader-security concern and Gerald Combs's macOS CI report qualify an otherwise merged relocation design. !9610 was subsequently reverted and is not a preferred implementation over !9793; !9607 needed !9621 to restore macOS plugin lookup.
- **!9592, !9593, !9581, !9583, !9562:** check both literal field lengths and metadata widths, duplicate hf abbreviations, and checker regex flexibility; run project checkers and inspect code semantically. Martin Mathieson's precise warnings are strong evidence.
- **!9579:** dissector offset and state changes must not depend on whether an optional protocol tree is requested; a first-pass/second-pass consumed-length discrepancy is a regression signal.
- **!9586:** when initial protocol capabilities are missing from a midstream capture, separate evidence-supported fallback inference from optional user-configured assumptions. Alexis La Goutte suggested a preference, but this MR did not implement one.
- **!9600 (closed):** maintain translated strings through the project's Transifex workflow, not a one-off direct .ts submission. This is authoritative review guidance, not a merged change.
- **!9572:** tests relying on private Python unittest internals need compatibility for Python 3.11 and older Python; MR states pytest remains a separate caveat.

## Continuation

Next expected candidate: **!9561**, subject to rebuilding the explicit reviewed set if the corpus or notebook changes. The recurring automation must remain enabled and unpaused.
