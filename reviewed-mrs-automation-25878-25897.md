# Supplemental Reviewed Wireshark Merge Requests — !25878–!25897

This is a continuation of `reviewed-mrs-automation.md`. Consult this file together with `reviewed-mrs.md` and `reviewed-mrs-automation.md` when selecting unreviewed MRs.

Corpus commit: `a67598e5dd77b04de676ab8cee5e2754525edf8e`

| MR | Status | Notes |
|---|---|---|
| !25878 | Scanned | Merged UET fix adds the missing `UET_SES_OPCODE_TSEND_ATOMIC` case to the standard, small, and medium SES request paths. Merged by John Thacker; no substantive human review beyond approval. |
| !25879 | Scanned | Merged Guy Harris-authored DCT2000 comment/indentation cleanup. Strong readability/style provenance, but no new general convention beyond existing consistency guidance. |
| !25880 | Scanned | Merged release-4.6 backport of the QCustomPlot tracer overflow fix. Keeps intersection geometry in `QRectF` rather than narrowing large floating-point coordinates to integer `QRect`. |
| !25881 | Scanned | Merged release-4.4 backport of the same QCustomPlot `QRectF` overflow fix. No additional lesson. |
| !25882 | Scanned | Merged Qt Enabled Protocols wording cleanup makes singular/plural terminology consistent. UI-only. |
| !25883 | Scanned | Merged recent-state compatibility fix validates stored column format against the current preference column before reusing a persisted width, falling back to the default width on mismatch. Useful compatibility pattern but not promoted as a broad rule. |
| !25884 | Deep | Merged NMEA0183 parametric-message expansion. Anders Broman explicitly said the initial change was too large to review; Pascal Quantin had to provide consolidated feedback because the GitLab diff was too large for normal UI review. Pascal also recommended Wireshark helpers such as `ws_strtou32()` and noted the unsigned semantics of `tvb_captured_length_remaining()`. Promoted the reviewability lesson to `review-patterns.md`. |
| !25885 | Scanned | Merged c-ares dependency update to 1.34.8 following a security release/ABI correction. Build dependency maintenance; no reusable review lesson. |
| !25886 | Deep | Merged Follow Stream charset fix exports and reuses Wireshark's `get_string_enc_iconv()` so malformed source byte sequences are replaced locally with U+FFFD and conversion continues instead of dropping the entire buffer. Promoted to `architecture.md` as robust shared text-conversion guidance. |
| !25887 | Scanned | Merged Qt capitalization consistency fix for “Display as Packet Details”. UI-only. |
| !25888 | Scanned | Merged Export Packet Dissections support for UTF-8 CSV with optional BOM. Notes an existing escaped-column representation constraint; no substantive human review establishing a broader convention. |
| !25889 | Scanned | Merged UET SES_REQ_MEDIUM correctness fix advances the offset by the actual two-byte `req_length` width rather than three bytes. Straightforward offset correction. |
| !25890 | Scanned | Merged DTLS 1.3 fix passes the correct end offset to `tls_scan_server_hello()`, restoring version detection and decryption. Assisted-by Claude; no substantive human review beyond approval. |
| !25891 | Scanned | Merged NAS 5GS registration-wait-range fix decodes the second timer at `offset+1` and labels it as the maximum rather than repeating the minimum timer. |
| !25892 | Scanned | Merged NAS 5GS extended CAG information-list fix introduces the correct CAG-ID list origin before applying entry-length arithmetic. Good offset-origin example but no broader review lesson. |
| !25893 | Scanned | Merged release-4.6 backport of !25891. No additional lesson. |
| !25894 | Scanned | Merged release-4.4 backport of !25891. No additional lesson. |
| !25895 | Scanned (closed draft) | Large NVMe decode/window-hardening draft with added regression tests. Pipeline failed in the newly added tests; the author closed the draft and stated that a new MR would be opened after resolving the failures. Down-weighted as an abandoned/superseded work-in-progress; no implementation convention promoted. |
| !25896 | Scanned | Merged NAS 5GS UE-security-capability fix advances the consumed offset and then runs the established extraneous-data check. Reinforces exact consumed-length accounting without adding a new rule. |
| !25897 | Scanned | Merged John Thacker GTP refactor moves extension-header dissection into the table, recognizes both old/current Long PDCP PDU Number values, and handles the reported-bounds failure generically. Strong implementation provenance but no substantive review discussion requiring a new notebook rule. |
