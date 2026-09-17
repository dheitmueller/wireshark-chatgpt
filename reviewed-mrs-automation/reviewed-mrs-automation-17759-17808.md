# Automated Wireshark MR review: !17759-!17808

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Review direction: descending MR number (newest toward older).

## Exact reviewed set

!17808, !17807, !17806, !17805, !17804, !17803, !17802, !17801, !17800, !17799,
!17798, !17797, !17796, !17795, !17794, !17793, !17792, !17791, !17790, !17789,
!17788, !17787, !17786, !17785, !17784, !17783, !17782, !17781, !17780, !17779,
!17778, !17777, !17776, !17775, !17774, !17773, !17772, !17771, !17770, !17769,
!17768, !17767, !17766, !17765, !17764, !17763, !17762, !17761, !17760, !17759.

Count: **50**.

The historical !17571-!17620 batch remains reviewed and must continue to count when constructing the already-reviewed set.

## Review weighting and durable findings

Merged MRs were treated as stronger evidence than closed/abandoned/superseded proposals. Maintainer-authored and maintainer-approved fixes were weighted accordingly.

Notable accepted examples from this batch include:

- **!17808** (`epan: Fixup adjusting length of FT_PROTOCOL value`): merged. Separates the operation of changing an `FT_PROTOCOL` value's length from setting the protocol value itself. A NULL tvb is a legitimate nullable value and therefore must not be overloaded as an implicit control flag. This reinforces the existing API-design rule to represent distinct operations explicitly rather than stealing valid domain values as sentinels.
- **!17807** (`ORAN FH CUS: Add Section Type 9`): merged. Straightforward protocol-extension work; no additional durable review convention identified.
- **!17780** (`HTTP2: Fix decompression of single padded DATA frames`): merged, John Thacker. The immediate dissector fix constrains the tvb passed to DATA-body handling to the actual data length, while explicitly documenting that the generic one-fragment reassembly path should eventually return a correctly bounded tvb itself. Useful boundary lesson: consumers should receive a view bounded to the semantic payload, especially when padding follows it; a local containment fix may be appropriate while the shared abstraction is separately corrected.
- **!17770** (`Remove remaining format specifiers from expert summaries`): merged, Martin Mathieson. Removes printf-style format specifiers from registered expert summaries and enables the expert-info check in the broad static-check set once the tree is clean. This reinforces the established pattern that mechanically enforceable conventions should become automated checks after false-positive/legacy debt has been removed.
- **!17760** (`Qt: Prevent overflow in I/O Graph simple moving average`): merged, John Thacker. Tracks moving-average positions directly as indices instead of repeatedly scaling interval values, eliminating an overflow path introduced when interval resolution increased by 1000x; also rejects negative indices before array access. Reinforces existing arithmetic-safety guidance: keep calculations in the representation actually needed by the algorithm and validate indices at the access boundary.

No new convention file was added in this run because these lessons corroborate existing API-design, bounded-data, static-analysis, and arithmetic-safety guidance rather than establishing a distinct new convention.

## Continuation

For the next run, rebuild the reviewed set from all ledgers plus `reviewed-mrs.md`; do not infer completeness from numeric ranges alone. Continue downward by selecting the fifty highest-numbered corpus MRs not present in that rebuilt set.