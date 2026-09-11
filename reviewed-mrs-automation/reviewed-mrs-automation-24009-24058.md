# Wireshark MR Review Automation: !24009–!24058

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection was made from the exact union of entries in `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, and the available per-run ledgers under `reviewed-mrs-automation/`. Numeric filename ranges were not treated as proof that every MR in a range was reviewed. The historical !17571–!17620 batch remains preserved and counted.

Exactly 50 previously unreviewed corpus MRs were reviewed in descending order: !24058 through !24009 inclusive.

| MR | Review status | Notes |
|---|---|---|
| !24058 | Discussion-focused, closed draft | Proposed E.212 “Follow IMSI” support reused Follow-stream infrastructure. Jaap Keuter rejected the premise because that infrastructure operates on actual transport protocols represented in `frame.protocols`; author acknowledged. Retained as negative API-domain evidence, not promoted as accepted architecture. |
| !24057 | Scanned, closed | Very broad first-contribution “Dissector API v2” migration touched roughly 1,800 files and failed CI; closed without acceptance. Down-weighted heavily. |
| !24056 | Scanned, open draft | Experimental GitLab shared-runner macOS packaging work by Gerald Combs. Still draft/unmerged, so not used for durable build guidance. |
| !24055 | Scanned, merged | One-line Qt Learn-card padding adjustment; presentation-only. |
| !24054 | Deep/corroboration, merged | John Thacker documents that BT HCI ISO’s custom reassembly behaves like missing `fragment_add_next()` and fails to mark depended-upon frames. Strongly corroborates the existing rule to prefer Wireshark’s standard reassembly infrastructure. |
| !24053 | Deep/corroboration, merged | John Thacker hardens MKA/MACsec state and decryption: file-scoped SAK state, diagnostics for unsupported E/C combinations, conservative refusal to decrypt when metadata is insufficient, duplicate-fetch cleanup, and overflow fixes. Reinforces existing state-lifetime, conservative-decoding, fetch-once, and arithmetic guidance. |
| !24052 | Scanned, merged | CIP device-profile value table expansion, approved and merged by Anders Broman; protocol-data maintenance only. |
| !24051 | Discussion-focused, merged | CIP STRINGI/STRINGN support and tree/filtering improvements. Michael Mann’s review included Wireshark’s `_U_` annotation convention for unused parameters; useful coding-style confirmation but too narrow for a new standalone rule. |
| !24050 | Scanned, merged | NGAP PSCellInformation decoding correction; protocol-specific generated/dissector correctness. |
| !24049 | Deep/corroboration, merged backport | John Thacker backport keeps wiretap record `Buffer` logical length synchronized with appended data and corrects `g_strlcpy()` offset accounting under truncation. Reinforces existing authoritative-length/Buffer and capacity guidance. |
| !24048 | Scanned, merged | Clean resubmission of Diameter S6c enhancements after !24043; merged by Anders Broman. |
| !24047 | Scanned, merged | XnAP specification upgrade to v19.2.0; generated/protocol maintenance. |
| !24046 | Scanned, merged | NGAP specification upgrade to v19.2.0; generated/protocol maintenance. |
| !24045 | Scanned, merged | E1AP specification upgrade to v19.2.0; generated/protocol maintenance. |
| !24044 | Scanned, merged | F1AP specification upgrade to v19.2.0; generated/protocol maintenance. |
| !24043 | Scanned/down-weighted, closed | Earlier Diameter S6c submission superseded by the cleaner merged !24048. Retained only as submission-history context. |
| !24042 | Scanned, merged backport | Release-4.4 backport of the ZIP traversal fix represented by !24035. |
| !24041 | Scanned, merged backport | Release-4.6 backport of the ZIP traversal fix represented by !24035. |
| !24040 | Deep/corroboration, merged backport | Guy Harris/John Thacker `wtap_read_bytes_or_eof_buffer()` helper ensures capacity and updates Buffer end state. Reinforces using abstraction-owned mutation APIs rather than raw buffer writes. |
| !24039 | Scanned, merged backport | Release-4.4 backport of RDP’s 65,535-byte uncompressed-segment limit check. |
| !24038 | Scanned, merged backport | Release-4.6 backport of RDP’s 65,535-byte uncompressed-segment limit check. |
| !24037 | Scanned, merged backport | SBC endian correction avoids forcing big-endian output and then manually swapping; preserves platform/library endianness contract. |
| !24036 | Scanned, merged | O-RAN tap separates beam counts by direction; tap/statistics correctness without a new cross-cutting rule. |
| !24035 | Deep, merged | Qt ZIP import hardening prevents malicious archive paths from traversing outside the extraction destination. Immediately targeted for both 4.4 and 4.6 backports. Promoted to `archive-extraction-conventions.md`. |
| !24034 | Scanned, merged | PROFINET SXP refactoring plus field/dissection additions; no distinct cross-cutting convention. |
| !24033 | Deep/corroboration, merged | TTL use-after-free fix separates the object retained as packet `proto_data` from the object inserted into a hash table, clarifying ownership roles. Reinforces existing ownership/lifetime discipline. |
| !24032 | Deep/corroboration, merged backport | Wiretap formats migrated from raw Buffer-array mutation to APIs that update `first_free` automatically. Reinforces the existing rule that container metadata must remain authoritative. |
| !24031 | Deep/corroboration, merged | Guy Harris encapsulates EyeSDN append logic in a buffer-aware helper and uses `ws_buffer_end_ptr()` rather than the start pointer for append operations. High-authority corroboration of Buffer abstraction ownership. |
| !24030 | Scanned, merged backport | Doxygen documentation for `buffer.h`; documentation-only. |
| !24029 | Scanned, merged backport | Release-4.4 iLBC double-free fix. |
| !24028 | Scanned, merged backport | Release-4.6 iLBC double-free fix. |
| !24027 | Scanned, merged backport | Cosine parser uses `%hh` to scan directly into byte-sized storage instead of wider ints plus casts; narrow type/API correctness. |
| !24026 | Scanned, merged backport | Checksum helper validates that flag combinations requiring `computed_checksum` cannot receive NULL, addressing UB/programmer error. Reinforces API-precondition guidance. |
| !24025 | Deep/corroboration, merged | Master iLBC double-free fix; resource-ownership correction with stable backports !24028/!24029. |
| !24024 | Scanned, merged backport | Release-4.6 backport of SDP dynamic-payload ownership fix represented by !24018. |
| !24023 | Scanned, merged | TCP RST diagnostic payload updated to draft-17 after the specification removed the free-description format; specification-following protocol maintenance. |
| !24022 | Deep/corroboration, merged | Master RDP parser enforces the protocol’s 65,535-byte uncompressed-segment maximum before processing. Reinforces protocol-bound validation before buffer operations. |
| !24021 | Scanned, merged backport | Release-4.4 AMR-NB mode-7 overflow fix. |
| !24020 | Scanned, merged backport | Release-4.6 AMR-NB mode-7 overflow fix. |
| !24019 | Deep/corroboration, merged | Master AMR-NB bandwidth-efficient conversion fixes placement of a final partial byte and prevents overflow; arithmetic/capacity correctness. |
| !24018 | Deep/corroboration, merged | SDP avoids freeing shared/reused RTP dynamic-payload memory when INVITE/200 OK control attributes coincide; ownership/lifetime fix with stable backport !24024. |
| !24017 | Scanned, merged | Automatic registry/manuf/service/translation update for master; no reusable engineering lesson. |
| !24016 | Scanned, merged | Automatic registry/data update for release-4.6; no reusable engineering lesson. |
| !24015 | Scanned, merged | Automatic registry/data update for release-4.4; no reusable engineering lesson. |
| !24014 | Deep, merged | Michael Mann factors repeated CMake plugin boilerplate into semantic macros for dissector, tap, codec, and wiretap plugins while retaining the generic primitive for uncommon and third-party plugins. Promoted to `plugin-build-conventions.md`. |
| !24013 | Discussion-focused, merged | Stratoshark preference check demotes an expected missing-module message from warning to noisy. Michael Mann questioned why the path ran at all; Gerald Combs noted an early exit/application-flavor guard might be cleaner but the change was tactical for a demo. Recorded as review context, not a durable architecture rule. |
| !24012 | Deep/corroboration, merged | Guy Harris adds `wtap_read_bytes_or_eof_buffer()` so capacity growth and Buffer end-pointer updates happen inside the helper; strong corroboration of abstraction-owned mutation. |
| !24011 | Deep/corroboration, merged | Master SBC endianness correction relies on the codec API’s native/default sample-endian contract instead of forced endian plus manual swapping; validated against an existing RTP capture. |
| !24010 | Deep/corroboration, merged backport | SBC decode loop breaks on zero/negative decoder returns and decrements remaining input/output capacities to avoid infinite loops and heap overflow. Reinforces strict progress and remaining-capacity accounting. |
| !24009 | Deep/corroboration, merged backport | Same SBC decode-loop hardening for release-4.6; stable propagation strengthens the existing progress/capacity rules. |

## Durable notebook effect

- Added `plugin-build-conventions.md`: !24014 establishes semantic, type-specific CMake wrappers for repeated plugin build contracts while preserving the generic helper when third-party compatibility or uncommon plugin types still require it.
- Added `archive-extraction-conventions.md`: merged !24035 plus accepted stable backports !24041/!24042 establish that archive-controlled member paths must remain contained within the selected extraction root.
- !24054, !24053, !24049, !24040, !24033, !24032, !24031, !24025, !24022, !24019, !24018, !24012, !24010, and !24009 primarily corroborate existing notebook rules on standard reassembly, scoped state, Buffer invariants, ownership, bounds, strict progress, and capacity; duplicate rules were not added.
- Closed !24058 is retained as useful negative evidence that an existing framework should not be reused when its semantic contract (transport protocols in `frame.protocols`) does not match the proposed object (IMSI). Because it was rejected and unmerged, it was deliberately not promoted as an accepted rule.
