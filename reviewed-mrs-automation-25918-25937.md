# Supplemental Reviewed Wireshark Merge Requests — !25918–!25937

This is a continuation of the reviewed-MR ledger. Consult this file together with `reviewed-mrs.md`, `reviewed-mrs-automation.md`, and earlier continuation ledgers when selecting unreviewed MRs.

Corpus commit: `a67598e5dd77b04de676ab8cee5e2754525edf8e`

| MR | Status | Notes |
|---|---|---|
| !25918 | Scanned | Merged release-4.6 backport of the UMTS-FP fixed-array hardening: stops MAC-is SDU tracking at the concrete 16-entry storage limit and avoids calling the MAC subdissector without required `data` context. Corroborates the existing implementation-capacity rule from !25780. |
| !25919 | Scanned | Merged release-4.4 backport of the same UMTS-FP fixed-array/context hardening as !25918. No additional lesson. |
| !25920 | Scanned | Merged Python tooling cleanup for `tools/pkt-from-core.py` addressing Ruff/Python 3 issues. Martin Mathieson noted the changes were straightforward despite not running the script; no broader convention extracted. |
| !25921 | Scanned | Merged release-4.4 backport of the Windows libgcrypt 1.12.2-3 package update. Dependency-package maintenance; no new review lesson. |
| !25922 | Deep | Merged ICMPv6 ND-family Hop Limit validation. Adds expert warning for invalid Hop Limit, tests field registration plus both valid/no-warning and invalid/warning cases, and narrowly changes `text2pcap` to emit Hop Limit 255 only for affected ICMPv6 families while preserving unrelated IPv6 behavior. Promoted positive/negative validation and narrowly scoped generator-regression guidance to `testing-fuzzing.md`. |
| !25923 | Scanned | Merged release-4.6 backport of !25917 K12 writer-capacity hardening. Corroborates the existing rule that reader capability does not imply writer capability. |
| !25924 | Scanned | Merged John Thacker GTPv2 cleanup replaces a post-increment sentinel `while` loop with a conventional indexed `for` loop, avoiding overflow warnings and improving readability. Narrow implementation cleanup; no new general rule. |
| !25925 | Deep | Merged John Thacker EAX hardening widens length parameters from 16 to 32 bits to match C12.22 callers, uses checked arithmetic for workspace sizing, and returns failure on unsupported/overflow cases. Strongly corroborates existing semantic-domain and checked-length guidance; no duplicate rule added. |
| !25926 | Scanned | Merged release-4.4 backport of !25917 K12 writer-capacity hardening. No additional lesson. |
| !25927 | Scanned | Merged release-4.6 ANSI TCAP registration fix adding missing `hf_` registrations in template/generated code. Straightforward correctness backport; no new general rule. |
| !25928 | Deep | Merged John Thacker ANSI TCAP cleanup fixes copied labels/filter names, a strict-inequality off-by-one, parameter-set item length, and removes an embedded newline from expert text because expert blurbs are enforced as single-line. Added the single-line expert-info convention to `dissector-conventions.md`. |
| !25929 | Scanned | Merged Windows manifest fix moves UTF-8 `activeCodePage` settings under the schema-correct application element using Microsoft documentation. Platform/resource-specific correction; no broader Wireshark rule. |
| !25930 | Deep | Merged John Thacker BLF writer hardening treats truncated Ethernet/VLAN records as `WTAP_ERR_UNWRITABLE_REC_DATA` rather than internal bugs, checks minimum bytes before field reads, and rejects payload lengths that cannot fit BLF's 16-bit representation. Strong corroboration of wiretap error taxonomy and writer-capability rules; noted in `architecture.md`. |
| !25931 | Discussion-focused | Merged Guy Harris wiretap format-name cleanup expands user-facing names, including vendor identity. Guy also suggested that duplicated opener/file-type naming metadata could eventually come from one format-registration source. High-authority architectural direction retained here, but the generation idea is explicitly future/aspirational and was not promoted as current convention. |
| !25932 | Deep | Merged release-4.6 EAX backport moves stateless decryption workspace from static storage to the `Eax_Decrypt()` stack and passes it explicitly to helpers, eliminating accidental cross-call/concurrency coupling. Promoted per-call scratch-state/reentrancy guidance to `architecture.md`. |
| !25933 | Scanned | Merged release-4.4 backport of the EAX static-to-stack workspace fix. Independently preserves the per-call scratch-state behavior captured from !25932. |
| !25934 | Scanned | Merged John Thacker spelling cleanup uses the already accepted `canonicalized` terminology/word-list entry. No reusable engineering convention beyond ordinary spelling hygiene. |
| !25935 | Deep | Merged John Thacker COSEM recursion fix keeps the dissection-depth guard active through the recursive child call and decrements only after the child returns; the previous evaluation order let nested arrays evade the configured depth limit. Promoted scoped recursion-depth accounting to `dissector-conventions.md`. |
| !25936 | Scanned | Merged release-4.6 backport of !25930 BLF truncated/oversized Ethernet writer hardening. Corroborates the master behavior without new discussion. |
| !25937 | Scanned | Merged release-4.4 backport of !25925 EAX length-width/checked-arithmetic hardening. Corroborates the master behavior without additional review evidence. |
