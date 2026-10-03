# Wireshark MR review run manifest: !559-!608

Corpus repository: `dheitmueller/wireshark-corpus-mrs`  
Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`  
Notebook predecessor: `automation/mr-review-609-658-authoritative` at `d245ab0e6a3a0096370a4d05de46973e7c52f12d`  
Model: GPT-5.6 Sol

## Selection validation

The reviewed set was selected by exact subtraction, not by assuming ranges. Tracking consulted included the persistent `reviewed-mrs.md`, the aggregate and per-run files under `reviewed-mrs-automation/`, and the complete available `automation/mr-review-*` branch inventory. The predecessor ledger explicitly says !608 was inspected only at metadata level and was not counted as reviewed. No completed low-number automation review branch exists below !609, making !608 the next frontier.

The historical !17571-!17620 batch was re-counted from its dedicated ledger: 50 rows, 50 unique MR numbers, no omissions.

Exactly 50 corpus MRs were reviewed, !608 down through !559 inclusive. All fifty JSON corpus records exist at the pinned corpus commit.

## Weighting

Merged master work is treated as the strongest implementation evidence. Stable backports primarily corroborate master behavior. Closed, superseded, draft, and immediately reverted work is not used as accepted implementation precedent, but explicit review from authoritative maintainers is retained when it clarifies an API or project convention.

## Status mix

42 merged; 8 closed/unmerged: !604, !594, !591, !590, !586, !585, !567, !562.

!573 is a special case: it merged, but Guy Harris immediately reverted it in !578. The final accepted state represented by !578 is weighted above !573.
