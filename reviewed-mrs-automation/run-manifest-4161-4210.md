# Run manifest: Wireshark MRs 4161–4210

- Model: GPT-5.6 Sol
- Corpus: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook repository: `dheitmueller/wireshark-chatgpt`
- Notebook predecessor branch: `automation/mr-review-4211-4260-authoritative-complete`
- Notebook predecessor commit: `b21548106c2f296def7104800d5afb8bcfec9fa8`
- Selection direction: descending MR number
- Exact batch: MR 4210 through MR 4161
- Reviewed count: **50**
- Unique reviewed count: **50**
- Merged: **47**
- Closed/unmerged: **3** (!4209, !4192, !4179)

## Tracking reconciliation

The predecessor notebook contained 677 entries under `reviewed-mrs-automation/`. The normal descending exact ledger frontier was MR 4211–4260. Root `reviewed-mrs.md`, `reviewed-mrs-automation.md`, and the available irregular exact-list/noncontiguous/gap/backfill/reconciliation trackers were checked for exact membership of MR 4161–4210. None contained a candidate from this batch.

The historical MR 17571–17620 exact ledger was re-opened and verified to contain all 50 unique numbers with no omissions.

## Run artifacts

Created:

- `reviewed-mrs-automation/reviewed-mrs-automation-4161-4210.md`
- `reviewed-mrs-automation/review-findings-4161-4210.md`
- `reviewed-mrs-automation/conventions-4161-4210.md`
- `reviewed-mrs-automation/run-manifest-4161-4210.md`

Durable conventions are promoted into the relevant topical notebook files in the same authoritative commit.

## Evidence weighting

Merged master changes are primary evidence. Stable-branch changes are corroborating unless they contain independent review. Closed/superseded MRs are lower-weight and are not treated as accepted implementation precedent. Direct Guy Harris implementation/review in this batch is given especially high weight.
