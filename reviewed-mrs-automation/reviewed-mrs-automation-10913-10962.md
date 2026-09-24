# Automated MR review ledger: !10962 through !10913

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Notebook starting point for this run: `65dffdf9952df38418c3f74b4c4fab3d8c8e9ba1`

The reviewed set was reconstructed from the available per-run files under `reviewed-mrs-automation/` together with `reviewed-mrs.md`. The exact-list tracker left by the preceding !11012 through !10963 run was included, and the historical !17571 through !17620 batch remains counted. Candidate numbers were checked individually against the tracking set; no numeric interval was assumed to be complete merely because neighboring MRs appeared in a ledger.

## Exact reviewed MR set

!10962, !10961, !10960, !10959, !10958, !10957, !10956, !10955, !10954, !10953,
!10952, !10951, !10950, !10949, !10948, !10947, !10946, !10945, !10944, !10943,
!10942, !10941, !10940, !10939, !10938, !10937, !10936, !10935, !10934, !10933,
!10932, !10931, !10930, !10929, !10928, !10927, !10926, !10925, !10924, !10923,
!10922, !10921, !10920, !10919, !10918, !10917, !10916, !10915, !10914, !10913.

Exactly 50 MRs were reviewed.

State summary: 47 merged; 3 closed/unmerged (!10922, !10921, !10917).

Merged master work was weighted most heavily. Stable backports were treated as corroboration of their master change. Closed/unmerged work was retained only as lower-weight process or negative evidence.

## Durable findings

- **!10961 — deep, merged master, authored/merged by John Thacker.** UAT invalidation flags represent independent effects: rebuilding the named-field environment does not itself imply packet redissection. Display-filter macros need `UAT_AFFECTS_FIELDS` without `UAT_AFFECTS_DISSECTION`. Promoted to `configuration-invalidation-conventions.md`; previously reviewed stable counterparts !10978 and !10977 corroborate the same behavior.
- **!10916 — deep, merged master, substantive Pascal Quantin review.** E.212 MCC/MNC values are identifiers whose leading zeros and fixed width are semantically significant. Integer fields irreversibly lose that identity in display filters and `tshark -e`; the accepted change deliberately switches both related fields to strings after compatibility discussion. Promoted to `field-value-semantics-conventions.md`.
- **!10920 — deep, merged master.** UDP multicast statistics are refactored so shared analysis code exposes callbacks/results and returns registration failure to its caller, while Qt owns presentation/rescan behavior and sharkd can serialize the same analysis as JSON. Promoted to `frontend-analysis-boundary-conventions.md`.
- **!10956 — discussion-focused, merged but corrected by later !10982.** John Thacker identified that deduplication resources were initialized during application startup before preferences were available and could remain allocated if no capture occurred. The later merged lifecycle fix !10982 remains the stronger accepted architecture evidence.
- **!10949 — deep/corroboration, merged master.** The common streaming-reassembly helper gains real `DESEGMENT_ONE_MORE_SEGMENT` support and documents callback progress requirements. This is primary implementation history for the sentinel behavior already represented by later !11184/!11268 in `reassembly-conventions.md`; no duplicate rule was added.
- **!10945 — deep/corroboration, merged master, John Thacker.** Introduces the common `ENC_BOM` modifier so UTF-16/UCS-2/UCS-4 decoding can honor an on-wire BOM while retaining an explicit fallback byte order. Later reviewed !23895 demonstrates reuse of this facility, so no duplicate encoding rule was added.
- **!10937 — high-authority corroboration, merged master, Guy Harris.** libpcap remote-device enumeration is not inherently Windows-only; feature availability can depend on how a dependency is built on UNIX as well. This reinforces the existing portability rule to reason from actual API/build capability rather than a broad OS-family assumption.
- **!10951 — corroboration, merged master, Gerald Combs with John Thacker review.** Continues the migration from GLib scalar aliases to standard C/C99 types while explicitly recognizing dependency API boundaries such as GLib's own `gboolean` contracts. Existing `c-type-conventions.md` already covers this rule more strongly.
- **!10953 — merged, review-practice corroboration.** Alexis La Goutte requested a capture reproducing the HAProxy TLV problem; the contributor generated and attached a PII-free capture. Reinforces the established representative-capture expectation.
- **!10955 — merged, unresolved follow-up discussion retained.** Stig Bjørlykke noted that repeatedly injecting TLS secrets can create duplicate Decryption Secrets Blocks; John Thacker agreed deduplication is desirable, while provenance-aware handling remained unresolved. Later accepted secrets/DSB work is stronger evidence, so this was not promoted separately.
- **!10917 — closed/unmerged, down-weighted.** Discussion around removing the Windows packaging preparation target records Gerald Combs's concern that package steps must not rebuild binaries after signing. Because the proposal was abandoned, it is process context rather than an implementation exemplar.

The remaining MRs were scanned for their diffs, affected paths, commit intent, and available discussion. They were routine documentation, CI/build maintenance, narrow protocol fixes, release backports, or changes already covered more strongly by existing notebook rules, so no additional durable convention was promoted.

## Notebook maintenance

This run also repaired notebook state from the preceding run: the proper !11012 through !10963 ledger now contains its exact reviewed set, and the durable packet-context, borrowed-view lifetime, and pending-lifecycle lessons that had been prepared on the prior automation branch are included in the notebook update commit.

Notebook convention commit prepared for this run: `9e0aab5a8ce592b7603faa85c0fd01a7a0e374ab`.

## Frontier

!10912 exists at the same corpus commit, is merged, and was checked only to establish the next descending frontier. It was not reviewed or counted in this batch.

The corpus has not run out.
