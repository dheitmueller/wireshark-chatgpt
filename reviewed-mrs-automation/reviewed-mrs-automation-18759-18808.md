# Wireshark MR review automation — !18759–!18808

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Selection method: rebuilt the already-reviewed set from the available `reviewed-mrs-automation/` ledgers and `reviewed-mrs.md`, preserving/counting the historical !17571–!17620 batch. Selected the fifty highest-numbered corpus MRs not in that set. The resulting batch is contiguous !18808 down through !18759. No numeric range was treated as reviewed merely from a ledger filename.

## Exact reviewed set

!18808, !18807, !18806, !18805, !18804, !18803, !18802, !18801, !18800, !18799,
!18798, !18797, !18796, !18795, !18794, !18793, !18792, !18791, !18790, !18789,
!18788, !18787, !18786, !18785, !18784, !18783, !18782, !18781, !18780, !18779,
!18778, !18777, !18776, !18775, !18774, !18773, !18772, !18771, !18770, !18769,
!18768, !18767, !18766, !18765, !18764, !18763, !18762, !18761, !18760, !18759.

Count: **50**.

## Review notes

Merged MRs were weighted above abandoned/superseded work. Diffs and discussions were inspected for reusable coding, architecture, testing, review, and submission conventions; maintainer-authored/reviewed work was weighted accordingly.

- **!18786 (merged, Jaap Keuter; merged by Anders Broman):** removes an `if (tree)` fence around MPLS PW ACH parsing so expert diagnostics are produced independently of whether a protocol tree is being rendered. This is strong corroboration of the notebook's existing rule that tree presence is a presentation concern and must not suppress semantic parsing, expert information, taps, or subdissector behavior.
- **!18780 / !18771 / !18765 (merged):** a sequence of LZ4 file-wrapper fixes around incomplete block-size reads, fast seeks, and compression-context reset boundaries. Collectively they reinforce the existing state/lifecycle rule: seek/restart behavior must restore decoder state at the semantic compression boundary, and short/incomplete input must terminate cleanly rather than continuing with partially established state.
- **!18763 (merged):** works around c-ares static-build target/library naming behavior. This is useful build-system corroboration that dependency discovery must match the actual linkage variant and exported target/library semantics rather than assuming shared/static naming is interchangeable.
- **!18808 (merged, Martin Mathieson):** O-RAN FH CUS ST11 refinements were tested with generated Scapy traffic. This corroborates the existing testing guidance that protocol changes should be exercised with concrete traffic and targeted vectors, even when captures are generated rather than externally sourced.
- **!18800 (merged):** LTE-RRC generated-field-name deduplication is further evidence that generated dissector output still has to satisfy the same unique/filter-safe field registration constraints as hand-written dissectors.
- **!18804:** RTPS type-alias support was inspected as protocol-extension work; it did not add a new cross-project convention beyond the notebook's existing schema/extension guidance.
- Documentation-only, mechanical, generated-data, and narrowly protocol-specific MRs in the batch were scanned but down-weighted where they supplied no durable cross-project lesson.

## Notebook impact

No new convention file was added or modified in this run. The strongest lessons above independently corroborate conventions already represented in the notebook, especially tree-independent semantic dissection, decoder-state lifecycle handling, dependency/linkage correctness, and traffic-backed protocol testing. Avoiding duplicate guidance is preferable to adding redundant rules.

Continuation: after rebuilding the reviewed set on the next run, continue with the highest-numbered unreviewed corpus MR; absent newly discovered gaps, the descending frontier is below !18759.
