# Run manifest 208-257

- Model: GPT-5.6 Sol
- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook base: `automation/mr-review-258-308-authoritative`
- Notebook result branch: `automation/mr-review-208-257-authoritative`
- Direction: descending MR number, newest previously unreviewed first
- Reviewed count: 50
- Exact range: !257 through !208, inclusive
- Outcome: 49 merged; 1 closed/unmerged (!251)
- Historical !17571-!17620 batch: revalidated as 50 unique reviewed MRs and preserved
- VANC/ST 291 types encountered: none

Before selection, the run consulted `reviewed-mrs.md`, the aggregate automation tracker, the per-run tracking directory, the automation review-branch inventory, and the predecessor exact ledger. The candidate set was checked against aggregate tracking content rather than inferred from a numeric-range assumption; no completed-review hit was found for !257-!208.

Merged master changes are primary evidence. Stable-branch cherry-picks/backports are corroborative unless their discussion adds independent guidance. Closed/superseded work is not implementation precedent. Maintainer-authored or maintainer-reviewed changes receive greater weight, particularly Guy Harris's protocol/core API work.
