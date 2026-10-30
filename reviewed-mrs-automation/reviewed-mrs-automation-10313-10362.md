# Wireshark MR review ledger — !10362–!10313

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`
Notebook prior head: `108d5a052d2450bc365feacb933baab1681e45af`
Reviewed date: 2026-10-30
Exact reviewed count: **50** (44 merged; 6 closed/unmerged)

## Selection and duplicate exclusion

The review tracking directory was inventoried (329 ordinary range-named per-run ledgers and 15 other Markdown tracking files) together with `reviewed-mrs.md`. The noncontiguous/gap ledgers and aggregate tracking were consulted as well as the preceding exact ledger `reviewed-mrs-automation-10363-10412.md`, which explicitly identified !10362 as the still-unreviewed frontier. The preserved `reviewed-mrs-automation-17571-17620.md` contains **50 distinct rows** and was counted as reviewed, not revisited.

The pinned corpus tree contains 26,575 MR JSON records (maximum !26580, with five missing numbers). Tracking inventory and exact exceptional MR memberships cover the available higher-numbered records, and the fifty highest-numbered previously unreviewed corpus entries are !10362 down through !10313. Selection was by individual membership, **not** by assuming coverage from a neighboring ledger range. These fifty JSON records, including their diffs and discussion notes, were inspected. No more than fifty were reviewed.

## Exact MR numbers

`!10362, !10361, !10360, !10359, !10358, !10357, !10356, !10355, !10354, !10353, !10352, !10351, !10350, !10349, !10348, !10347, !10346, !10345, !10344, !10343, !10342, !10341, !10340, !10339, !10338, !10337, !10336, !10335, !10334, !10333, !10332, !10331, !10330, !10329, !10328, !10327, !10326, !10325, !10324, !10323, !10322, !10321, !10320, !10319, !10318, !10317, !10316, !10315, !10314, !10313`

## Per-MR review notes

| MR | State | Depth | Finding |
| --- | --- | --- | --- |
| !10362 | merged | Deep | wmem integer lookup/contains now accept a null tree, returning absent rather than dereferencing; enables lazy allocation. |
| !10361 | merged | Deep | John Thacker adds a separate registered H.264 Annex B byte-stream entry point and short-NAL guard; payload splitting still assumes complete NALs. |
| !10360 | closed | Scanned | Unmerged WSML/XML export proposal across dissector and UIs; no accepted implementation precedent. |
| !10359 | merged | Deep | Replace two-state value strings with FT_BOOLEAN and shared TFS only when a genuine nonzero bit mask exists; retain bit polarity. |
| !10358 | merged | Scanned | HI2Operations ASN.1 source upgraded to ETSI 101 671 v3.15.1 with EPS handling; standards-specific regeneration. |
| !10357 | merged | Scanned | Guy Harris stable-branch ASN.1 SPNEGO template follow-up; keeps generated output reproducible. |
| !10356 | merged | Scanned | Second stable-branch SPNEGO template follow-up, same canonical-source rule. |
| !10355 | merged | Scanned | Guy Harris SPNEGO Kerberos returned offset correction, release backport. |
| !10354 | merged | Scanned | Parallel stable SPNEGO Kerberos offset fix; returned child offset is relative to child tvbuff. |
| !10353 | merged | Deep | Guy Harris fixes ASN.1 SPNEGO template after generated-file-only offset fix. |
| !10352 | merged | Deep | Guy Harris repairs ILP ASN.1 conformance source for value-to-string modernization. |
| !10351 | merged | Deep | Guy Harris repairs ATN-ULCS ASN.1 template for checker-discovered field issues. |
| !10350 | merged | Discussion-focused | H.264 access unit delimiter field; Alexis La Goutte flagged a duplicate/typo in its value map, author fixed. |
| !10349 | merged | Scanned | Guy Harris makes -1 protocol ID explicitly represent an unowned dissector table/list. |
| !10348 | merged | Scanned | packet.h documentation spelling cleanup; CI/approval workflow no new general rule. |
| !10347 | merged | Deep | John Thacker consolidates DVB/MPEG current-next bitfield displays using FT_BOOLEAN and common TFS. |
| !10346 | merged | Discussion-focused | X.509 NTDS OIDs; Alexis flagged CI whitespace and Stig traced generated trailing line to the template. |
| !10345 | closed | Discussion-focused | Abandoned broad SRT/time-shift protocol rewrite; John Thacker recommends redissection on time shifts instead. |
| !10344 | merged | Scanned | MAPI ROP input/output buffer parsing; accepted protocol-specific support via IDL/CNF and regenerated code. |
| !10343 | merged | Deep | val_to_str_const cleanup; Guy Harris catches generated packet-ilp.c edit and requires ASN.1 input repair. |
| !10342 | merged | Deep | MPEG wiretap preserves detected audio/PES mode, separates framing parsers, and skips permissible zero stuffing. |
| !10341 | merged | Deep | Typed-item checker corrections; Guy Harris requires generated ATN-ULCS source changes in template. |
| !10340 | merged | Scanned | Falco bridge case statement fix; localized correctness cleanup. |
| !10339 | merged | Scanned | dftest --refs exposes compiled display filter runtime reference structures; compile-only tool cannot fill runtime references. |
| !10338 | merged | Scanned | Display-filter implementation local-variable cleanup. |
| !10337 | merged | Scanned | Display-filter implementation missing-header dependencies fixed. |
| !10336 | merged | Scanned | Guy Harris refreshes CDP documentation URLs and restores an archived reference. |
| !10335 | closed | Scanned | Automated translation/registry refresh missed publication window; not merged. |
| !10334 | closed | Scanned | Automated tool-help refresh missed publication window; not merged. |
| !10333 | closed | Scanned | Another failed/out-of-window automated help refresh; not implementation evidence. |
| !10332 | merged | Deep | Invalid-argument recoverable-return logs moved to information severity and dedicated InvalidArg domain so fuzzers do not abort by default. |
| !10331 | merged | Deep | MPEG opener documents short-signature collision/false-positive risks; despite magic bytes it remains heuristic. |
| !10330 | merged | Deep | SPNEGO Kerberos child offset fix; Guy Harris identifies generated-source mismatch and submits template corrections. |
| !10329 | merged | Deep | Compiler diagnostic gating distinguishes actual GCC from Clang and Intel Classic/LLVM and uses a minimum GCC version. |
| !10328 | merged | Scanned | Wiretap developer guide updated for current file-type registration mechanism. |
| !10327 | merged | Scanned | MPEG/MPEG-TS extensions synchronized across file-open filters and heuristic registration. |
| !10326 | merged | Scanned | CMS ASN.1 template trailing-line correction restores regenerated-file parity. |
| !10325 | merged | Scanned | Qt I/O Graphs layout spacer fix. |
| !10324 | closed | Discussion-focused | Contributor withdrew hex-array clipboard feature after Stig identified existing Show Packet Bytes > C Array, including Rust mode. |
| !10323 | merged | Scanned | MPEG PES stream_id specification/reference comment correction in ASN.1 source and output. |
| !10322 | merged | Deep | MPEG program_end_code is a four-byte terminator with no PES length or payload; wiretap and dissector agree. |
| !10321 | merged | Scanned | MC-NMF ADWS dissector default port registration for TCP/9389. |
| !10320 | merged | Scanned | Fix GDT/Z39.50 encoding flags in ASN.1 templates after prior generated-only change. |
| !10319 | merged | Scanned | RDP capability-set parsing and frame-acknowledge capability support. |
| !10318 | merged | Deep | GCC diagnostic pragma for stringop-overflow needs GCC >=7.1; reviewer directs use of existing version helper. |
| !10317 | merged | Scanned | ITS ASN.1 country code rendered as two five-bit ITA-2 characters. |
| !10316 | merged | Scanned | Protobuf zero-length Timestamp message can encode default epoch fields; do not assume nonzero encoded content. |
| !10315 | merged | Discussion-focused | Expand typed-item macro substitution; author notes topic branch accidentally based on spell branch. |
| !10314 | merged | Scanned | NNTP null guard release backport. |
| !10313 | merged | Scanned | NNTP null guard master change. |

## Durable notebook lessons promoted

1. **ASN.1 generated-source integrity** — Guy Harris directly identified changes to generated `packet-*.c` files without the underlying ASN.1 template or conformance update in !10330, !10341, and !10343; accepted corrective MRs !10351–!10353 and !10356–!10357 restore source/output parity. Also corroborated by !10320 and !10326. Recorded in `generated-code-conventions.md`.
2. **Compiler diagnostic feature gates** — accepted !10318 and !10329 separate GCC version from GNU-compatibility macros; John Thacker explicitly called out both Intel compiler generations and the correct `__clang__` spelling. Recorded in `compiler-feature-selection-conventions.md`.
3. **True/false bitfield registration** — accepted !10347 and !10359 use `FT_BOOLEAN` plus shared `TFS` for genuine masked one-bit fields, keeping display polarity and storage width correct. Recorded in `field-mask-semantics-conventions.md`.
4. **Wiretap opener mode and MPEG framing** — accepted !10342 commits audio versus PES identity into reader state; !10331 documents why a short MPEG signature remains heuristic; !10322 treats the MPEG program-end marker as a zero-payload four-byte record; !10327 keeps opener extensions consistent. Recorded in `wiretap-file-detection-conventions.md`.
5. **Recoverable argument diagnostics and fuzzers** — accepted !10332 uses a dedicated `InvalidArg` logging domain at info level instead of making recoverable precondition failures fatal during fuzzing. Recorded in `logging-conventions.md`.

**Discussion retained but not promoted as accepted implementation:** !10345 was closed after John Thacker recommended redissection when timestamps are shifted, avoiding a broad per-dissector SRT rewrite. !10324 was withdrawn after a reviewer pointed to an existing GUI path with the requested functionality. Closed automated refreshes !10333–!10335 do not carry implementation authority.

## Continuation

The next highest-numbered candidate is **!10312**, present in the same pinned corpus. The corpus is not exhausted; continue backward, while first checking for newly added high-numbered records and all existing ledgers. Do not disable the recurring job.
