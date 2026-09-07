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
| !25793 | Scanned | Megaco encoding typo fix: replaces `ENC_STR_NUM` (a mask) with the concrete `ENC_STR_HEX` value for `tvb_get_string_uint`. Merged; useful API-correctness example but no broader convention extracted. |
| !25794 | Scanned | UET TSS entropy-header offset fix. Merged; author supplied a focused pcap demonstrating both IP/Entropy/TSS and IP/UDP/TSS before/after behavior, reinforcing existing capture-based validation practice. |
| !25796 | Scanned | Geneve GCP/FOPiC timestamp option. Merged; included example pcap and expected text output plus manual verification. Reinforces existing submission/testing guidance without adding a new rule. |
| !25798 | Discussion-focused | BGP MUP SAFI draft update. Alexis La Goutte requested a pcap; contributor supplied one after merge. Independent corroboration of the already high-confidence sample-capture expectation for protocol changes. |
| !25799 | Discussion-focused (closed) | Couchbase GET_EX/GET_EX_REPLICA support was submitted from fork `master`. Alexis instructed the contributor to close it and reopen from another branch. Reinforces the established named-topic-branch workflow rule; closed/unmerged, so not an implementation exemplar. |
| !25801 | Scanned | Windows libgcrypt bundle update to 1.12.2-3 to avoid repeated DLL load/unload behavior. Merged; dependency-package maintenance with no substantive human review. |
| !25802 | Scanned | release-4.6 backport of !25801's libgcrypt package update. Merged; no additional lesson. |
| !25804 | Scanned | Clean resubmission/successor of the Couchbase GET_EX/GET_EX_REPLICA change after the earlier fork-master MR was closed. Merged; confirms the implementation itself was straightforward once branch hygiene was corrected. |
| !25805 | Scanned | Qt QPointer optimization using C++14 init-capture and a Qt-version guard to move when Qt >= 6.6 supports the desired constructor behavior. Merged; no reusable dissector/review convention extracted. |
| !25806 | Deep | rlogin invalid-text fix. Replaces raw fixed-buffer `tvb_memcpy()` string handling with encoding-aware `tvb_get_string_enc(..., ENC_ASCII)` in file-scope memory so invalid characters are sanitized before later column formatting. Added packet-derived string/column safety guidance to `dissector-conventions.md`. |
| !25807 | Deep | LBM SRS heuristic safety fix authored by John Thacker. Explicitly states that heuristic dissectors must not throw exceptions for packets that do not belong to them; replaces unsigned subtraction-based length tests with `tvb_captured_length_remaining()`. Added as a durable heuristic-dissection rule. |
| !25808 | Scanned (closed backport) | First release-4.6 backport attempt of !25807. Closed after failed pipeline; superseded by merged !25809, so not an implementation exemplar. |
| !25809 | Scanned | Successful release-4.6 backport of !25807. Merged and independently reinforces the heuristic/no-exception and overflow-aware remaining-length rule. |
| !25811 | Scanned | release-4.6 backport of !25806 rlogin invalid-text fix. Merged; no additional lesson beyond the master change. |
| !25812 | Scanned | release-4.4 backport of !25806 rlogin invalid-text fix. Merged; no additional lesson beyond the master change. |
