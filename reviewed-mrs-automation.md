# Supplemental Reviewed Wireshark Merge Requests

This file supplements `reviewed-mrs.md` for recurring corpus-review runs. Consult both ledgers before selecting merge requests so already-reviewed MRs are not repeated. Entries here may later be folded into the main ledger.

## Corpus commit 2ed161d1

Corpus commit: `2ed161d19398e620fa89227724ed928d4f5edf27`

| MR | Status | Notes |
|---|---|---|
| !25788 | Scanned | release-4.6 backport of the ERF time-tag bounds fix already represented by !25786. Merged; validates tag length before fixed-size copy, with no new substantive review evidence. |
| !25789 | Scanned | release-4.4 backport of the same ERF time-tag bounds fix. Merged; no additional lesson beyond !25786. |
| !25790 | Scanned | Wireshark 4.7.2 / Stratoshark 0.10.2 build and release-note preparation. Merged with CI intentionally skipped; release-maintenance plumbing, no substantive human review. |
| !25791 | Scanned | Version preparation for Wireshark 4.7.3 / Stratoshark 0.10.3, including project/library version updates. Merged with CI intentionally skipped; no reusable engineering lesson. |
| !25792 | Scanned | Release-note initialization for 4.7.3 / 0.10.3. Merged; no substantive human review or additional convention. |
