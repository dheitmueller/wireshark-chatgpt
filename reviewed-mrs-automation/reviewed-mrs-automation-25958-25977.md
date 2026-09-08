# Supplemental Reviewed Wireshark Merge Requests — !25958–!25977

This is a continuation of the reviewed-MR ledger. Consult this file together with `reviewed-mrs.md`, `reviewed-mrs-automation.md`, and earlier continuation ledgers when selecting unreviewed MRs.

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit used for this batch: `a67598e5dd77b04de676ab8cee5e2754525edf8e`

Exactly twenty MRs were reviewed in this batch: **!25958 through !25977 inclusive**.

| MR | Review depth | Outcome / durable relevance |
|---|---|---|
| !25958 | Scanned | Merged release-4.4 backport of the KNX/IP `tvb_get_ptr()` correction. Corroborates the existing rule that a validated byte count is passed as a length, not reduced by the offset again. |
| !25959 | Scanned | Merged release-4.6 CMS backport. Corroborates reset-before-parse for transient ASN.1 state and explicit handling of a missing digest algorithm. |
| !25960 | Scanned | Merged ORAN FH CUS fix for uncompressed IQ handling when `iq_width != 16`. Protocol-specific arithmetic correction; no broader convention promoted from this MR alone. |
| !25961 | Scanned | Merged release-4.4 CMS backport of !25955/!25959. Further corroborates clearing stale parser state before sequence decoding and tolerating absent algorithm state. |
| !25962 | Scanned | Merged release-4.6 RTPDump backport. Corroborates `caplen` versus original `len`, checked mandatory-header subtraction, and `WTAP_ERR_BAD_FILE` for malformed input. |
| !25963 | Scanned | Merged release-4.4 RTPDump backport of the same correction. Counted as corroboration, not a separate architectural lesson. |
| !25964 | Deep | Merged John Thacker H.245 fix. A `returnedFunction` is treated like encapsulated/quoted traffic; when outer H.245 packet state has intentionally been cleared, nested GenericMessage dissection must not update the outer Info column or frame label. Useful evidence for context-sensitive state updates, but not generalized beyond existing state-isolation guidance. |
| !25965 | Scanned | Merged John Thacker X.509AF fix. Malformed certificates may omit the expected subject name, so optional parser-derived strings must be checked before C string operations such as `strstr()`. This is fundamental defensive parsing and did not require a new standalone notebook rule. |
| !25966 | Scanned | Merged release-4.6 backport of !25964. Corroborates suppressing outer-state updates while dissecting a returned/encapsulated H.245 function. |
| !25967 | Scanned | Merged release-4.4 backport of !25964. Same corroboration; no additional lesson counted. |
| !25968 | Scanned | Merged release-4.6 backport of the X.509AF null-subject fix. Corroborates optional/malformed-input null checking before string helpers. |
| !25969 | Discussion-focused | Merged WSDG documentation fix adding a reassembled-data field to the documented fragment example. Stig Bjørlykke immediately noted that he wanted the field removed because it duplicates another reassembled-data field and is apparently only used by TCP. Because the architectural direction remained contested even though the doc patch merged, no durable convention was promoted. |
| !25970 | Deep | Merged John Thacker Kerberos security fix. Replaces unconditional reads of four key bytes in expert-message formatting with a bounded helper that prints up to four bytes and adds an ellipsis only when the actual key is longer. Promoted to `parser-api-conventions.md`: diagnostic/expert formatting must obey actual buffer lengths. |
| !25971 | Discussion-focused | Open, conflicted Tagging Rules feature with substantial unresolved design discussion. Stig Bjørlykke argued that tagging/emoji semantics differ from coloring rules (all matches versus first match, column values versus row coloring, independent configuration) and therefore should be a separate feature. Because the MR is still open with unresolved discussions and conflicts, its design is intentionally not treated as accepted architecture. |
| !25972 | Scanned | Merged Couchbase support for encryption-key management opcodes. Extends opcode-specific extras/key/value/vbucket semantics consistently across existing dispatch functions. Protocol-specific extension; no new general rule. |
| !25973 | Discussion-focused | Merged WSLua Python/ruff cleanup. John Thacker pointed to CI's I001 import-order warning when the author's local ruff version did not reproduce it; the MR was revised and merged. Useful evidence to trust project CI/tooling configuration over assumptions from a differing local linter version, but not promoted as a Wireshark-specific coding rule. |
| !25974 | Scanned | Merged release-4.6 backport of the Kerberos bounded key-preview fix. Corroborates !25970. |
| !25975 | Scanned | Merged release-4.4 backport of the Kerberos bounded key-preview fix. Corroborates !25970. |
| !25976 | Deep | Merged John Thacker Bluetooth HFP security fix. Validates the fixed-width `AT+XAPL` parameter before fixed-offset hexadecimal parsing; malformed short values get expert info and parsing stops safely. Promoted to `parser-api-conventions.md`: validate fixed-format text width before raw indexing/substrings. |
| !25977 | Discussion-focused | Merged LBMC TSNI Request Info-column refinement with a dedicated test capture and explicit verification that ordinary RXREQ packets remain unchanged. Review also required updating ancillary Python code after rebasing. Useful example of feature-specific positive/regression validation, but no new testing convention beyond existing notebook guidance. |

## Notebook changes from this batch

`parser-api-conventions.md` was updated to:

- add release-4.4 !25958 as further corroboration of `tvb_get_ptr()` offset/length semantics;
- add !25959 and !25961 as release corroboration of reset-before-parse transient state handling;
- add !25962 and !25963 as release corroboration of RTPDump `caplen`/`len` and malformed-header handling;
- add a durable rule from !25976 to validate fixed-format text length before raw fixed-offset parsing;
- add a durable rule from !25970, corroborated by !25974/!25975, that diagnostic/expert previews must clamp reads to the actual buffer length.

Open or architecturally contested !25971 was deliberately down-weighted and retained only as review evidence rather than accepted Wireshark architecture.
