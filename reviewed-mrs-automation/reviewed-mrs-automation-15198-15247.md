# Wireshark MR review automation — 2026-09-20

Model: GPT-5.6 Sol

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `130eea3c8a60902c0fc98c9e1130e73bafc6f815`

## Selection and tracking

Before selecting this batch, the already-reviewed set was reconciled against the available tracking in `dheitmueller/wireshark-chatgpt`: `reviewed-mrs.md`, the supplemental automation ledger, the per-run ledgers under `reviewed-mrs-automation/`, and the immediately preceding exact run ledger. The historical !17571-!17620 batch remains explicitly preserved and counted. Selection used exact MR membership rather than assuming that a numeric range was complete merely because some members appeared in a ledger.

The immediately preceding exact run reviewed !15297 through !15248. Repository-wide tracking checks found no prior review ledger entry for the candidate range below !15248, and all fifty corpus records !15247 through !15198 are present and valid. These are therefore the fifty highest-numbered previously unreviewed corpus MRs for this run.

## Exact MRs reviewed in this run

!15247, !15246, !15245, !15244, !15243, !15242, !15241, !15240, !15239, !15238

!15237, !15236, !15235, !15234, !15233, !15232, !15231, !15230, !15229, !15228

!15227, !15226, !15225, !15224, !15223, !15222, !15221, !15220, !15219, !15218

!15217, !15216, !15215, !15214, !15213, !15212, !15211, !15210, !15209, !15208

!15207, !15206, !15205, !15204, !15203, !15202, !15201, !15200, !15199, !15198

Count: **50 MRs**.

Status weighting: **48 merged**, **2 closed/unmerged** (!15217 and !15209). The two closed proposals were down-weighted and were not used as positive implementation precedent.

## Durable findings promoted to the notebook

### Keep flow-graph endpoint positions stable across traffic direction

Merged master !15245 fixes same-address conversations in Flow Graph and VoIP sequence views. John Thacker explicitly requested a direction-independent layout: keep the two endpoint ports in a stable relative order (he suggested smaller port on the left) and show packet direction by reversing the arrow rather than swapping endpoint positions. His review also found a source/destination-node mix-up that was corrected before merge.

Added `flow-graph-layout-conventions.md`.

Notebook commit: `a633221f34deed322378a0724259608a1220605d`

### Bound reused transaction identifiers with independent context

Merged master !15240, authored and merged by John Thacker, adds an optional maximum request/reply matching interval to PFCP so a reused sequence number in a long capture need not pair with a stale transaction. Jaap Keuter's review led to checking sequence equality before computing the optional timestamp delta; John noted analogous GTP/GTPv2 code.

Added `transaction-matching-conventions.md`.

Notebook commit: `a8abcbaa6ce513950abc12028987212401201506`

### Separate byte order from semantic bit significance

Merged master !15223, authored by John Thacker and merged by Alexis La Goutte, fixes IEEE 1905 HE-MCS masks. Reordering the field into big-endian byte order did not reverse which semantic field belongs to the LSB versus the MSB. John separately explained that tree display order can still put the MSB field first for consistency. Merged release backport !15224 independently carries the same correction.

Extended `wire-encoding-conventions.md`.

Notebook commit: `a91d7726b835f815c5aa007a77a4b89f5c345adb`

### Re-audit Qt AutoConnection after event-loop and lifetime refactors

Merged release MRs !15210 and !15211, authored and merged by John Thacker, carry the Time Shift crash fix. Removing a nested dialog event loop changed an AutoConnection into synchronous direct delivery; redissection could then outlive a `WA_DeleteOnClose` dialog and return into deleted widgets. Explicit `Qt::QueuedConnection` restores the required deferred boundary.

Extended `qt-signal-connection-conventions.md`.

Notebook commit: `71f93df3d5c31bc90d8f8972c7e882f07008b876`

### Validate wiretap structural length relationships and propagate scan errors

Merged release MRs !15204 and !15205 were authored and merged by Guy Harris and therefore receive especially high authority. The BLF reader now validates header/object length relationships before subtraction/seeking, distinguishes malformed and unsupported cases, checks seek failures, propagates `err`/`err_info` from its scan helper, and unwinds open-time state on failure.

Added `wiretap-reader-validation-conventions.md`.

Notebook commit: `89d4b50a7c82db9bf1f8670dbbfca9ec6153ae86`

### Long-running Qt operations need one surviving cancellation/lifetime owner

Merged master !15198, authored by John Thacker and merged by Anders Broman, hides the Export Dissections dialog once export begins. Its Cancel button did not cancel the export but could delete dialog state still needed at completion; the actual cancellation owner was the main-window progress dialog. Merged backports !15212 and !15213 carry the same correction.

Added `qt-long-operation-lifetime-conventions.md`.

Notebook commit: `8d6dab31659cf711a6c8fd7db18e596982231e64`

## Strong corroborating evidence retained without duplicating existing rules

- !15247 and !15246 continue the move from GLib aliases to standard C99/C types in Wireshark-owned code and in generator sources/output, reinforcing the existing type and generated-code conventions rather than creating another rule.
- !15237 is a compact Coverity-driven example of overflow-before-widen: `(int64_t)(interval * ui_interval)` is too late if the multiplication overflows as `int`; the accepted fix widens an operand before multiplication. This corroborates the notebook's existing checked/widen-before-arithmetic guidance.
- !15233 avoids treating frame number zero as a real PDCP-NR security-setup frame, reinforcing the existing rule that unset sentinels must remain outside the valid semantic domain or be tested explicitly before use.
- !15226 replaces ISO15765's effectively global/latest first-frame reassembly state with state keyed by bus type, frame ID, and interface ID. It strongly corroborates the existing state/reassembly rule that identity must include every independent flow dimension that can coexist.
- !15221 combines HTTP robustness and memory-lifetime cleanup with clang static-analyzer feedback; useful evidence for checking analyzer findings against actual ownership/nullability paths, but it did not warrant a separate convention.
- !15217 was closed after its CI experiment failed and is down-weighted as abandoned implementation evidence. !15209 was also closed/unmerged and was not promoted.
- The BLF sequence !15208 through !15203 contains several accepted format-correctness and bounds fixes. Guy Harris's !15204/!15205 pair supplied the strongest generalizable reader-validation evidence and was therefore promoted rather than creating overlapping rules for each sibling fix.
- !15199 and !15202 are accepted SSH HMAC fixes for the same underlying state bug; they provide correctness evidence but no additional durable convention beyond existing state-management guidance.

## Continuation

`mr_15197.json` exists in the same corpus commit and is a valid merged MR (`BLF: Properly skip 0-length containers`). The corpus is therefore not exhausted; the next run should continue selection from !15197 downward, still subtracting the exact reviewed set rather than assuming contiguous range coverage.
