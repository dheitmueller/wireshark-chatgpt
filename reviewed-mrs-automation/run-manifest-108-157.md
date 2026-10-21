# Run manifest: Wireshark MR review !108-!157

- Model: GPT-5.6 Sol
- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook predecessor: `automation/mr-review-158-207-authoritative` at `1e05fd9309d7cf58e3bf9ef554a6775232f58ce7`
- Reviewed count: 50
- Reviewed MRs: !157 through !108 inclusive
- Outcomes: 49 merged; !127 closed/unmerged
- Historical !17571-!17620 preserved count: 50 unique MRs
- Next metadata-only frontier probe: !107, `tools: Force "Allow commits from members..." in merge requests.`; merged on master; authored by Gerald Combs
- ST 291/VANC encountered: no

## Tracking reconciliation

The `reviewed-mrs-automation/` directory inventory was inspected before selection. Exact per-run ledgers cover the preceding low-number batches, including `ledger-158-207.md`. `reviewed-mrs.md` and the supplemental aggregate tracker contained no completed-review row for any of !157-!108. The historical !17571-!17620 tracker contains exactly 50 unique MRs and remains counted.

Selection was done by individual MR number, not by assuming that any numeric range was fully reviewed merely from a range-like ledger filename.

## Corpus verification

The corpus Git tree at the recorded commit was inspected. Each of `mr_157.json` through `mr_108.json` exists and has non-zero size. Corpus `main` was rechecked after analysis and still pointed at `ddcaa22b51c68f594e425a23388c3a2086813054`.
