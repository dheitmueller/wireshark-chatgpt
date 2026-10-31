# Wireshark MR review ledger — !9311 through !9262

Reviewed 2026-10-31. Corpus repository: `dheitmueller/wireshark-corpus-mrs` at exact commit `ddcaa22b51c68f594e425a23388c3a2086813054`. Notebook initial main head: `0d0b937e7be381f32dddcee999370fef59318415`.

**Exact membership:** !9311, !9310, !9309, !9308, !9307, !9306, !9305, !9304, !9303, !9302, !9301, !9300, !9299, !9298, !9297, !9296, !9295, !9294, !9293, !9292, !9291, !9290, !9289, !9288, !9287, !9286, !9285, !9284, !9283, !9282, !9281, !9280, !9279, !9278, !9277, !9276, !9275, !9274, !9273, !9272, !9271, !9270, !9269, !9268, !9267, !9266, !9265, !9264, !9263, !9262.

**Count:** 50 distinct MRs in descending order, !9311–!9262 inclusive; **41 merged, 8 closed without merging, 1 open (!9279)**. The closed set is !9311, !9310, !9309, !9308, !9307, !9299, !9295, !9294.

## Tracking and selection

Before selection, consulted the notebook's root `reviewed-mrs.md`, supplemental `reviewed-mrs-automation/reviewed-mrs-automation.md`, the complete per-run tracking **inventory** (360 files in `reviewed-mrs-automation/` before this run), and earlier explicit-membership reconciliation, including the prior run's !9361–!9312 ledger and noncontiguous/exception ledgers. The prior run had reconciled the full per-run set and established !9311 as the next frontier at the same unchanged corpus commit. Special membership formats checked include the otherwise placeholder !10963–!11012 ledger with its companion exact-list TMP, the MR-prefixed !10463–!10512 list, plain-number !19940–!19989 entries, noncontiguous !17559–!17658 and the exception/gap ledgers. Root tracking is not interpreted as whole reviewed ranges: only explicit reviewed entries count. All selected 50 JSON files exist and the corresponding discussions/diffs were inspected. No additional previously-reviewed candidate was identified.

**Historical preservation:** The untouched `reviewed-mrs-automation/reviewed-mrs-automation-17571-17620.md` explicitly lists all **50 distinct members !17571 through !17620**, and all 50 remain counted as previously reviewed.

## Exact 50-MR assessments

| MR | Corpus outcome | Depth | Evidence / assessment |
| --- | --- | --- | --- |
| !9311 | closed | Scanned | OPC UA DiagnosticInfo reorder attempt reverted itself; zero surviving changed files. Closed and unmerged, not an implementation exemplar. |
| !9310 | closed | Scanned | Same OPC UA fix accidentally compared against a wrong base: 502 changed files for a one-file intended fix. Closed; negative branch/diff scope example. |
| !9309 | closed | Scanned | Closed focused repeat of OPC UA Locale/LocalizedText decode order correction, not accepted. |
| !9308 | closed | Scanned | Closed repeat of OPC UA Locale/LocalizedText field-order correction; not an independent accepted precedent. |
| !9307 | closed | Scanned | Closed repeat of the same OPC UA correction; no substantive reviewer approval. |
| !9306 | merged | Deep | John Thacker adapts Python unittest _outcome handling for 3.11; CTest works but pytest integration explicitly remains broken. |
| !9305 | merged | Scanned | Registers DTLS dissector for SCTP PPIs carrying 3GPP NGAP, XnAP, F1AP, E1AP. |
| !9304 | merged | Scanned | Martin Mathieson adds O-RAN section extension values and associated decoding. |
| !9303 | merged | Deep | João Valverde repairs generated Flex unused-parameter diagnostic regression via balanced, narrow warning controls; Alexis requests accurate commit scope and analyzer follow-up. |
| !9302 | merged | Deep | João Valverde removes unused arithmetic commute mode from dfilter semantic checking and updates negative syntax tests. |
| !9301 | merged | Deep | João Valverde requires a letter in protocol filter name, not just any nondigit, to avoid grammar collisions. |
| !9300 | merged | Deep | Metamako trailer heuristic/preferences expansion; Alexis requests pcap and identifies Windows abs warning; revised by author. |
| !9299 | closed | Discussion-focused | Closed OPC UA stable-branch proposal; Alexis explicitly says submit on master first, then backport to release-4.0. |
| !9298 | merged | Deep | Martin Mathieson restores FT_UINT32/VALS MBIM numeric enums after Pascal Quantin rejects overbroad FT_BOOLEAN conversion in !9281. |
| !9297 | merged | Scanned | Gerald Combs release-3.6 NSIS donation page backport of existing mainline work. |
| !9296 | merged | Scanned | Gerald Combs release-4.0 NSIS donation page backport. |
| !9295 | closed | Scanned | Empty closed draft release-3.6 preparation; no source precedent. |
| !9294 | closed | Scanned | Empty closed draft release-4.0 preparation; no source precedent. |
| !9293 | merged | Scanned | Gerald Combs release-3.6 IAX2 Qt dialog Coverity fix frees dfilter object on early return. |
| !9292 | merged | Scanned | Gerald Combs release-4.0 counterpart of IAX2 dialog early-return filter cleanup. |
| !9291 | merged | Scanned | Gerald Combs release-3.6 Valgrind uninitialized-state hardening in BEEP, GSM, ROHC, Telnet. |
| !9290 | merged | Scanned | Release-4.0 counterpart of same Valgrind initialization changes. |
| !9289 | merged | Scanned | Release-3.6 ChmodBPF postinstall removes copying of nonexistent macOS plist path. |
| !9288 | merged | Scanned | Release-4.0 counterpart of ChmodBPF installer path fix. |
| !9287 | merged | Scanned | Gerald Combs release-3.6 ElidedLabel escaping of plain setter data entering Qt rich-text renderer. |
| !9286 | merged | Scanned | Release-4.0 counterpart of ElidedLabel HTML-escaping fix; later caller integration !9462 already reviewed. |
| !9285 | merged | Deep | Gerald Combs release-3.6 TIPC recursion depth guard, expert warning and normal-path depth restoration. |
| !9284 | merged | Scanned | Release-4.0 TIPC recursion guard backport. |
| !9283 | merged | Deep | Martin Mathieson scopes bitmask checker to actual add_bitmask fields arrays and detects overlapping masks; LAT/NFS/RTLS corrections. |
| !9282 | merged | Scanned | DECT NWK spelling fixes, protocol-specific. |
| !9281 | merged | Deep | Over-broad numeric-to-Boolean TFS conversion; Pascal Quantin rejects MBIM case, corrected in merged !9298. |
| !9280 | merged | Scanned | CQL release-4.0 backport of signed sentinel-length correction !9266. |
| !9279 | open | Deep | Open TCP payload-root proposal. Guy Harris questions inconsistent packet-record hierarchy; author considers pcapng-boundary solution; Anders asks revision. |
| !9278 | merged | Scanned | Stig Bjørlykke fixes CQL 64-bit row number formatting to PRId64, correcting !9268. |
| !9277 | merged | Scanned | Stig Bjørlykke corrects AVSP 64-bit control-word formatting to PRIx64. |
| !9276 | merged | Discussion-focused | HTTP/3 Datagram/WebTransport additions; Alexis requests sample capture and suggests splitting distinct features. |
| !9275 | merged | Scanned | João Valverde makes dfilter semantic debugging level/format consistent. |
| !9274 | merged | Deep | João Valverde makes one-byte dfilter byte literal unambiguous (fc:), aligns lexer and formatter, adds syntax regression. |
| !9273 | merged | Deep | Zstd TVB leak: single owning tvb_new_real_data with g_free callback, empty-input regression; avoids composite wrapper leaks. |
| !9272 | merged | Deep | Flex scanner cleanup removes old warning suppressions; post-merge RHEL7/GCC4.8.5 warnings repaired and verified in !9303. |
| !9271 | merged | Scanned | Automated 2023 registry/documentation update, master. |
| !9270 | merged | Scanned | Automated 2023 registry/documentation update, release-4.0. |
| !9269 | merged | Scanned | Automated 2023 registry/documentation update, release-3.6. |
| !9268 | merged | Scanned | CQL adds row labels using nonportable %ld with 64-bit counter; fixed by !9278. |
| !9267 | merged | Scanned | 2023 copyright-year refresh for release-4.0. |
| !9266 | merged | Deep | CQL distinguishes short unsigned byte lengths from signed -1 NULL/-2 unset sentinel values; only advances on positive lengths. |
| !9265 | merged | Scanned | 2023 copyright-year refresh for release-3.6. |
| !9264 | merged | Discussion-focused | 2023 copyright-year refresh, master; Gerald asks whether annual year refresh is needed; later !9468 closed. |
| !9263 | merged | Scanned | CQL labels LZ4 and Snappy decompressed data sources distinctly. |
| !9262 | merged | Scanned | CQL adds decode of SUPPORTED opcode string multimap. |

## Durable findings promoted to notebook

- **!9281 → !9298 and !9336:** Pascal Quantin's post-merge MBIM correction establishes that two familiar enum values do not turn a full 32-bit protocol value into a Boolean; corrected accepted code restores `FT_UINT32`/`VALS`.
- **!9283:** Martin Mathieson's typed-item checker validates bitmask-array overlap **only** for arrays passed as bitmask fields, and catches real mask errors in LAT, NFS, and RTLS.
- **!9273:** Zstd decompression can transfer one GLib-owned contiguous output buffer into one owning real-data TVB; verify empty input and all cleanup paths rather than accumulating leaky child TVBs.
- **!9284/!9285:** Gerald Combs's TIPC backports use explicit per-protocol recursion limits and an expert warning. For new code, prefer current project-wide dissection-depth APIs.
- **!9272 → !9303:** João Valverde's removed Flex warning workarounds broke GCC 4.8.5/RHEL7 generated scanner builds; narrow balanced diagnostic suppression fixed it, as confirmed by Joakim Karlsson.
- **!9274 and !9301:** Display-filter literals and protocol field names must not collide lexically; `fc:` unambiguously denotes one-byte data.
- **!9306:** John Thacker's unittest private-API workaround was explicitly CTest-compatible, not proof that pytest worked.
- **!9266/!9280:** CQL signed lengths carry legitimate negative sentinels; never add them directly to offsets.
- **!9286/!9287 and previously reviewed !9462:** Plain-text Qt APIs backed by rich text escape input; style must come from explicit widget presentation APIs.
- **!9279 (open):** Guy Harris's tree-hierarchy objection is architecturally authoritative review input, but the proposed code is **not** an accepted solution.
- **!9299 and !9307–!9311 (closed):** Alexis La Goutte explicitly requests master-first/backport-later; the 502-file accidental compare is negative branch hygiene evidence.
- **!9277/!9278, !9300, !9276:** Fixed-width format macros, Windows builds, sample pcaps, and feature separation corroborate upstream portability and submission expectations.

**Evidence discipline:** Merged mainline and corrective follow-ups count more strongly than stable cherry-picks or abandoned proposals; strong direct maintainer feedback is separately captured even when its MR was closed or left open. No packet captures were analyzed and no builds, runtime dissection, CI, or fuzz tests were run during this corpus documentation review; GitLab state is from the pinned snapshot, not rechecked live.

## Continuation

Next expected candidate: **!9261**, conditioned on exact membership and any newly scraped higher-numbered records being rechecked before selection.
