# Automated Wireshark MR review: !17109-!17158

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Review direction: descending from newest toward older MRs. Before selecting this batch, existing per-run ledgers in `reviewed-mrs-automation/` and `reviewed-mrs.md` were consulted. The historical !17571-!17620 batch remains part of the reviewed set. This run selected the fifty highest-numbered corpus MRs not already represented by that tracking.

## Exact reviewed set

!17158, !17157, !17156, !17155, !17154, !17153, !17152, !17151, !17150, !17149,
!17148, !17147, !17146, !17145, !17144, !17143, !17142, !17141, !17140, !17139,
!17138, !17137, !17136, !17135, !17134, !17133, !17132, !17131, !17130, !17129,
!17128, !17127, !17126, !17125, !17124, !17123, !17122, !17121, !17120, !17119,
!17118, !17117, !17116, !17115, !17114, !17113, !17112, !17111, !17110, !17109.

Count: **50**.

## Review notes and weighting

Merged master changes were weighted most strongly. Release-branch cherry-picks, mechanical updates, and superseded/closed submissions were counted but given less independent evidentiary weight. Human review discussion was considered separately from system-generated approval/rebase notes; feedback from established maintainers was weighted according to authority and specificity.

Notable evidence:

- **!17158 (merged, master): Qt Copy as HTML.** Adds HTML clipboard output while deliberately retaining plain-text clipboard data and defaults that preserve existing copy/paste behavior. Useful compatibility/presentation evidence, but it reinforces existing compatibility guidance rather than establishing a new convention.
- **!17150 (merged, release-4.4): ID3v2 display-filter conflict fix.** Removes a duplicate `hf_id3v2` field and uses the protocol ID for the protocol-tree item. This is a backport and therefore lower-weight than its master change; it corroborates avoiding protocol/filter-name collisions and unnecessary duplicate registered fields.
- **!17140 (merged, master, Gerald Combs): GitLab CI vcvars environment handling.** Pipes `cmd.exe ... set` directly into PowerShell environment import instead of round-tripping through a temporary file. Clean CI simplification with no additional durable architectural rule.
- **!17130 (merged, release-4.4, John Thacker): ITS CPMv1 private-data/column fix.** Backport initializes per-packet private state and protocol/info columns for CPMv1. Later discussion also caught an incorrect security-advisory affected-version statement; useful reminder that release/security metadata must be checked independently from code correctness, but not enough evidence here for a new notebook rule.
- **!17120 (closed/superseded): AX.25 extended sequence-number mode.** The author supplied concrete tshark test commands but closed the MR because it was replaced by !17121 to satisfy pipeline/submission requirements. Treat !17121, not this superseded submission, as the implementation exemplar.
- **!17110 (merged, master, Gerald Combs): DTD grammar scan-build fix.** Duplicates `Name->text` before storing it so parser cleanup cannot free storage still owned by the resulting configuration. Strong corroboration for explicit ownership/lifetime separation across parser-generated objects; this is already covered by the notebook's memory-management guidance.
- **!17109 (merged, master, Gerald Combs): MATE scan-build fixes.** Marks an exception-throwing helper `WS_NORETURN` and guards a possibly-null parser result before dereference/insertion. This reinforces accurate control-flow annotations and defensive parser-boundary handling already represented in existing guidance.

No new convention file was added or modified in this run: the durable findings corroborate existing compatibility, ownership/lifetime, static-analysis, parser-boundary, and submission-scope guidance rather than establishing a distinct new rule.

## Continuation

Rebuild the already-reviewed set from all current tracking before the next run. If no newer gaps have appeared, the next descending candidate after this batch is **!17108**.
