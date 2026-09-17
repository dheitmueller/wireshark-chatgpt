# Automated MR review ledger: !17559-!17658 (non-contiguous selection)

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Review direction: descending MR number, selecting the highest-numbered corpus MRs not already represented by existing review tracking. The previously reviewed !17571-!17620 batch was preserved and counted as already reviewed, so this run deliberately skips it.

## Exact MRs reviewed in this run (50)

!17658, !17657, !17656, !17655, !17654, !17653, !17652, !17651, !17650, !17649, !17648, !17647, !17646, !17645, !17644, !17643, !17642, !17641, !17640, !17639, !17638, !17637, !17636, !17635, !17634, !17633, !17632, !17631, !17630, !17629, !17628, !17627, !17626, !17625, !17624, !17623, !17622, !17621, !17570, !17569, !17568, !17567, !17566, !17565, !17564, !17563, !17562, !17561, !17560, !17559.

## Weighting and durable findings

The corpus records, discussions, and diffs were reviewed with merged master changes weighted above release backports and closed/superseded work. Low-information backports and routine fixes were scanned rather than promoted into notebook rules.

- **!17658 (merged, master; John Thacker):** wmem tree removal had been implemented as a logical/fake removal that happened to work for exact `lookup32` but violated the semantics of `lookup32_le`, which could stop at the removed node instead of finding the next valid predecessor. The accepted change implements actual red-black-tree deletion/rebalancing and adds tests. Durable core-container lesson recorded in `memory-management-conventions.md`.
- **!17630 (merged, master; John Thacker):** UMTS RLC buffers allocated in `wmem_file_scope()` were still being released with `g_free()`, risking invalid/double frees when the file-scope allocator later cleaned up. The accepted fix matches `wmem_free()` to the allocation domain and also avoids `memcpy(NULL, ..., 0)` undefined behavior. Durable allocator/lifecycle lesson recorded in `memory-management-conventions.md`.
- **!17640 (merged release-4.4 backport):** Qt D-Bus is made optional because Wireshark remains functional without it, with reduced functionality. Useful dependency/configuration corroboration, but a backport and not distinct enough for a new rule.
- **!17650 (merged release-4.4 backport; Gerald Combs):** restores stapling of the notarization ticket to the Intel macOS DMG after an earlier CI change inadvertently dropped it. Packaging-specific regression; no general convention extracted.
- **!17621 (merged, master):** adds the Display Filter Reference to the Help menu and updates translations/tooltips. Useful UI integration example but no durable engineering convention beyond existing UI/localization practice.
- **!17570 (merged release preparation; Gerald Combs):** release-note/security-advisory preparation for 4.4.1; operational release work rather than an implementation exemplar.
- **!17559 (merged release-4.4 backport; Gerald Combs/John Thacker):** validates that subtracting a checksum trailer cannot make the DMP remaining length negative before later parsing. This reinforces existing defensive length-validation guidance and therefore did not need a new rule.

No abandoned or superseded MR in this batch was allowed to outweigh merged evidence. No new VANC capture evidence was encountered because this run reviewed MR corpus records rather than packet captures.

## Notebook changes

Added `memory-management-conventions.md` with two durable themes: allocator/deallocator domain matching (including allocator-migration cleanup audits and zero-length memory-operation pointer validity), and preserving the complete semantic contract of core containers when implementing removal/ordered lookup.

## Continuation

For the next run, rebuild the reviewed set from all ledgers rather than assuming contiguous ranges. In descending order, !17558 is the next candidate if it is present in the corpus and is not independently listed as reviewed elsewhere.
