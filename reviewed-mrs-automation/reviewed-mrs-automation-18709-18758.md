# Automated MR review: !18709–!18758

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Reviewed on 2026-09-16. Selection was made by rebuilding the already-reviewed set from `reviewed-mrs.md` and the accumulated files under `reviewed-mrs-automation/`, preserving and counting the historical !17571–!17620 batch. These are the fifty highest-numbered corpus MRs not already represented in that tracking at selection time. Review direction remains newest toward older.

## Exact reviewed set

!18758, !18757, !18756, !18755, !18754, !18753, !18752, !18751, !18750, !18749,
!18748, !18747, !18746, !18745, !18744, !18743, !18742, !18741, !18740, !18739,
!18738, !18737, !18736, !18735, !18734, !18733, !18732, !18731, !18730, !18729,
!18728, !18727, !18726, !18725, !18724, !18723, !18722, !18721, !18720, !18719,
!18718, !18717, !18716, !18715, !18714, !18713, !18712, !18711, !18710, !18709.

Count: **50**.

## Review notes

Merged MRs were weighted more strongly than drafts, abandoned work, or superseded proposals. Low-information automatic updates, duplicated-field cleanups, dependency bumps, spelling changes, and narrow release/build adjustments were scanned for discussion/diff significance but were not promoted merely to create notebook churn.

- **!18758 — Deep, merged.** SOME/IP configuration debugging initially attempted to use an internal text mechanism from dissector code. Anders Broman objected that `text_internal` is not intended for dissectors and suggested the logging system; Alexis La Goutte agreed. The merged direction uses the supported logging framework. This is strong corroboration of the existing `logging-conventions.md` rule to use Wireshark's logging facilities rather than internal/presentation mechanisms for diagnostics, so no duplicate rule was added.
- **!18749 / !18747 — Merged.** DNP3 reassembly fixes handle transport sequence reset at FIR and correctly treat a NULL TVB from `process_reassembled_data()` as meaning reassembly occurred in another frame. These reinforce existing reassembly/state guidance: use the reassembly API's return contract rather than inferring completion solely from protocol flags.
- **!18745 — Deep, merged.** Infiniband exposes enough RC Send PDU context for subdissectors and SMB Direct uses it for fragmentation/reassembly. The replacement was validated with the capture from the superseded approach. This corroborates keeping transport framing/reassembly capability in the owning lower layer while allowing the payload dissector to drive protocol-specific PDU reconstruction.
- **!18731 — Merged.** NTP registry updates add expert information for historic/erroneous extension assignments, reinforcing the existing practice of remaining able to dissect historical/nonconforming traffic while making the deviation visible rather than silently treating it as current-valid protocol.
- **!18723 — Merged tooling.** `tools/check_dissector.py` gains an `--open` mode and expands typed-item checks. This corroborates the notebook's existing preference for turning mechanically detectable dissector API mistakes into project tooling/pre-submit checks.
- **!18710 — Deep discussion, merged, but later evidence takes precedence.** SocketCAN attempted to recover old captures lacking proper CAN-FD identification using frame-length inference. Post-merge discussion strongly disputed the ambiguity when the lost metadata cannot be reconstructed. This line of work was subsequently revisited in !18831 and resolved by the later !18994 design already reviewed in a newer batch; therefore this MR is treated as historical context rather than a standalone convention source.
- **!18709 — Merged.** A QUIC test requiring Brotli is skipped when Brotli support is absent, reinforcing capability-sensitive test gating rather than treating optional build features as universally present.

No durable convention file was modified in this run because the strongest findings corroborate rules already present in the notebook or are superseded by stronger later evidence already reviewed. Avoiding redundant/contradictory notebook guidance is preferable to adding a rule for every batch.

## Continuation

Continue backward from the highest-numbered corpus MR not present in the rebuilt reviewed set; do not assume !18708 is necessarily next without checking all tracking for holes or previously reviewed out-of-order MRs.
