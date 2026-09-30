# Run manifest: !4111–!4160

- Model: GPT-5.6 Sol
- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook predecessor commit: `b5979f0e3c444bf444c19f7ccd4b20bf88f700c2`
- Review direction: descending from newest unreviewed MR toward older MRs
- Exact reviewed set: every MR from !4160 down through !4111 inclusive
- Count: 50 unique MRs
- Outcomes: 46 merged; 4 closed/unmerged
- Closed/unmerged: !4153, !4147, !4122, !4118
- Historical preservation check: `reviewed-mrs-automation/reviewed-mrs-automation-17571-17620.md` still records exactly !17571 through !17620 (50 unique MRs) and remains counted.

## Selection reconciliation

The predecessor notebook contained 681 entries under `reviewed-mrs-automation/`. The ordinary descending exact-range frontier ended with !4161–!4210. Candidate !4160–!4111 membership was checked against root `reviewed-mrs.md`, the aggregate `reviewed-mrs-automation/reviewed-mrs-automation.md`, and all available irregular gap/backfill/noncontiguous/exact-list/reconciliation/supplemental tracking files. None recorded any candidate in this batch as previously reviewed. Selection therefore used exact set subtraction rather than assuming the interval was unreviewed merely from the ledger filename frontier.

## Weighting

Merged master changes were treated as the strongest implementation precedent. Stable backports were used primarily as corroboration where the master-origin MR was present. Closed/unmerged submissions were retained only for reviewer guidance or negative design history. Guy Harris-authored/reviewed evidence received high weight, especially !4156, !4154, !4144, and the correction of !4134 by the later authoritative !4202.
