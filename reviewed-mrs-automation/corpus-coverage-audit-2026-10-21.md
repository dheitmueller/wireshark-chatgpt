# Corpus review-coverage reconciliation — 2026-10-21

**New Wireshark merge requests reviewed in this reconciliation: 0.**
**Corpus commit:** `ddcaa22b51c68f594e425a23388c3a2086813054`.
**Corpus records:** 26,575 distinct `mr_<number>.json` files, covering MR !1 through !26580 except !8696, !9604, !16948, !21132, and !21133, which are absent from the pinned corpus. No `mr_0.json` exists.

## Reconciliation outcome

The recursive corpus Git tree was compared with the exact reviewed-MR tracking across the notebook's `reviewed-mrs.md`, supplemental `reviewed-mrs-automation/reviewed-mrs-automation.md`, the historical/per-run review ledgers, and the authoritative low-number and backfill branches. The completed-review tracking accounts for all 26,575 **present** corpus MR records. Therefore no eligible previously unreviewed MR could be selected, and no MR diff or discussion was re-reviewed. No run ledger purporting to record new MR reviews was created.

The special !17571–!17620 batch remains preserved in `reviewed-mrs-automation/reviewed-mrs-automation-17571-17620.md` and was independently validated as exactly 50 unique reviewed MRs.

Past corpus read anomalies requiring exact backfills, not assumed range coverage, are !1956, !15376, !701, and !261. The corresponding completed-review ledgers are `ledger-backfill-1956.md` and `ledger-backfill-15376-701-261.md`. The !25759–!25827 mixed region is accounted for by exact MR rows in `reviewed-mrs.md`, `reviewed-mrs-automation.md`, and `reviewed-mrs-automation-25827-backfill-50.md`. The earlier !26365–!26393 review records are explicit in `reviewed-mrs.md`. Discontiguous and intentionally omitted ranges were not treated as fully reviewed merely on the strength of their filenames.

Four ledgers previously declared complete ranges but did not enumerate all members as `!N` tokens. This reconciliation **does not review those MRs again**; it adds machine-readable enumerations to the !5761–!5810, !6261–!6310, !6461–!6510, and !8661–!8711 (excluding absent !8696) historical ledgers, preserving their original text.

## Tracking consolidation and provenance

This branch imports previously committed, unchanged files under `reviewed-mrs-automation/` that were missing from the preceding repair branch. Their Git blob SHAs are preserved; no historic findings were rewritten. Sources:

- Consolidation base: `automation/mr-ledger-repair-5711-5760-20261017` at `6673be109d8f372a596a81769c707b583acf6e2e`.
- Completed low-number ledger lineage: `automation/mr-review-1-7-authoritative-final` at `a64be22ba4f274954ebd92d326b2d89239ab1913`.
- Latest review/backfill lineage: `automation/mr-review-backfill-15376-701-261-authoritative-final` at `422ab4cfb25f82bc24f338025a0a2bfc2ea579ab`.
- !1956 backfill lineage: `automation/mr-review-backfill-1956-authoritative` at `87c5094506ffb6690b4b23527dd09473a07db8c1`.

This is a tracking-only repair and inventory check, **not an additional review batch**. Future runs must still rebuild exact reviewed membership from actual ledger contents and compare it with the current recursive corpus tree, especially when the corpus commit changes.
