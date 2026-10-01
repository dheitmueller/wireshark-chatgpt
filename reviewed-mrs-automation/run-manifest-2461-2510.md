# Run manifest 2461-2510

- Model: GPT-5.6 Sol.
- Corpus repository: dheitmueller/wireshark-corpus-mrs.
- Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`.
- Notebook branch: `automation/mr-review-2461-2510-authoritative`.
- Selection direction: descending MR number from the newest available previously unreviewed frontier.
- Selected MRs: 2510 through 2461 inclusive, exactly 50.
- Outcomes: 48 merged; 2 closed/unmerged (2494 and 2473).
- The immediately preceding reviewed batch 2511-2560 was restored into the notebook as an exact per-run ledger because its prior review completed but repository writes were blocked during that run.
- Historical batch 17571-17620 remains preserved and counted as 50 reviewed MRs.
- Closed MRs were down-weighted relative to merged behavior and maintainer-authored merged fixes.
- No SMPTE ST 291/VANC packet type was encountered in this batch.
- Next frontier probe: MR 2460, `Dissection of Abort packet and characters number in Authorization`, merged on master; metadata only and not counted as reviewed.
- Corpus was rechecked at the same commit after selection; it is not exhausted.
