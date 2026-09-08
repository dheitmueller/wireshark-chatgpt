# Supplemental Reviewed Wireshark Merge Requests — !25898–!25917

This is a continuation of `reviewed-mrs-automation.md`. Consult this file together with `reviewed-mrs.md`, `reviewed-mrs-automation.md`, and earlier continuation ledgers when selecting unreviewed MRs.

Corpus commit: `a67598e5dd77b04de676ab8cee5e2754525edf8e`

| MR | Status | Notes |
|---|---|---|
| !25898 | Scanned | Merged release-4.6 backport of the NAS 5GS extended-CAG-list offset-origin fix represented on master by !25892. The CAG-ID loop measures `entry_len` from the correct conditional CAG-ID origin. No new review lesson beyond the master fix. |
| !25899 | Scanned | Merged release-4.4 backport of the same NAS 5GS extended-CAG-list fix. No additional lesson. |
| !25900 | Scanned | Merged AUTHORS maintenance removes Pascal Quantin's duplicate entry, updates his address, and incorporates another author entry. Metadata-only; no reusable engineering convention. |
| !25901 | Scanned | Merged release-4.6 backport of the NAS 5GS UE-security-capability consumed-length/extraneous-data fix represented by !25896. No additional lesson. |
| !25902 | Scanned | Merged release-4.4 backport of the same NAS 5GS extraneous-data fix. No additional lesson. |
| !25903 | Discussion-focused (open) | Proposed WSLua sandbox removes numerous dangerous standard-library calls but remained open with unresolved design discussion. Guy Harris questioned whether arbitrary file access plus upload still defeats the threat model and argued that trusted user-authored scripts likely need a whitelist/trust mechanism. High-authority security-design feedback, but down-weighted because no accepted implementation emerged. |
| !25904 | Deep | Merged NVMe/NVMe-MI decode expansion also hardens partial/windowed transfers: common field-in-window checks, correct logical-structure offsets, complete-record/alignment guards, exact-boundary fixes, and overflow-safe handling before forming `LPO + offset`. Promoted a durable partial/windowed-decoding rule to `dissector-conventions.md`. |
| !25905 | Scanned | Merged Qt Internals/Dissector Tables search enhancement replaces custom recursive filtering with Qt's recursive filtering/parent acceptance and adds separate name/description regex filtering. Useful framework reuse, but too UI-specific for a broader notebook rule. |
| !25906 | Discussion-focused (open) | SMB-over-QUIC proposal remained open. John Thacker requested a sample capture and, with Stefan Metzmacher, questioned whether the existing NBT/Direct-TCP framing path already handled the traffic and only needed its CIFS/24-bit-length context set correctly. Reinforces existing reuse-before-new-dissector and capture-evidence guidance, but is down-weighted while unmerged. |
| !25907 | Deep | Merged John Thacker Busmaster wiretap fix replaces Flex `YY_FATAL_ERROR()` on malformed capture syntax with `WTAP_ERR_BAD_FILE`, explanatory `err_info`, and local scanner termination. Promoted to `architecture.md`: attacker-controlled malformed file contents must use normal wiretap error returns, not terminate the process. |
| !25908 | Scanned | Merged UET SES_RESP_DATA_SMALL fix adds specification-defined reserved, job-ID, and original-request-PSN fields, with specification and before/after screenshots. Reinforces spec-aligned field coverage and visual validation without adding a new convention. |
| !25909 | Scanned | Merged Couchbase FBR snapshot/file-fragment opcode dissection. Anders Broman asked the contributor to investigate pipeline errors; after branch updates the change merged. No new durable convention beyond existing CI hygiene. |
| !25910 | Scanned | Merged assorted dissector-warning cleanup coincided with Ruff 0.16.0 suddenly enabling many more rules by default. Useful context for the CI mitigation in !25911, but no protocol/review convention extracted. |
| !25911 | Scanned | Merged John Thacker CI mitigation temporarily runs Ruff with `--exit-zero` after a tool release expanded default checks from 59 to 413, preventing unrelated work from being blocked while cleanup proceeds. Retained as CI-history evidence rather than promoting a universal rule from one tool incident. |
| !25912 | Scanned | Merged WiresharkXML.py cleanup replaces legacy Python 2-style print redirection with modern `print(..., file=...)` calls to satisfy current Ruff checks. Tooling maintenance; no broader lesson. |
| !25913 | Scanned (open draft) | Large draft adding multiple time scales remained open with ongoing discussion and no accepted outcome in this corpus snapshot. Down-weighted; no implementation convention promoted. |
| !25914 | Discussion-focused | Merged Vector ASC wiretap reader. Guy Harris noticed naming/description consistency with the existing Vector BLF format and the author agreed to align it. Useful high-authority consistency review, but too format-specific for a durable general rule. |
| !25915 | Scanned | Merged release-4.6 backport of !25907's Busmaster malformed-file error-path fix. Corroborates the master behavior without new discussion. |
| !25916 | Scanned | Merged release-4.4 backport of !25907's Busmaster malformed-file error-path fix. Corroborates the master behavior without new discussion. |
| !25917 | Deep | Merged John Thacker K12 writer hardening validates `caplen` against the dumper's actual frame buffer and returns `WTAP_ERR_UNWRITABLE_REC_DATA` with useful size details when the reader can accept a record larger than the writer can emit. Promoted to `architecture.md`: reader acceptance does not imply writer capability; reject unsupported output before copying. |
