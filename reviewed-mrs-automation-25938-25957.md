# Supplemental Reviewed Wireshark Merge Requests — !25938–!25957

This is a continuation of the reviewed-MR ledger. Consult this file together with `reviewed-mrs.md`, `reviewed-mrs-automation.md`, and earlier continuation ledgers when selecting unreviewed MRs.

Corpus commit: `a67598e5dd77b04de676ab8cee5e2754525edf8e`

| MR | Status | Notes |
|---|---|---|
| !25938 | Scanned | Merged release-4.6 backport of the EAX 32-bit length/checked-arithmetic hardening previously captured from the master change. Corroborates hostile length arithmetic and semantic-width rules; no new convention. |
| !25939 | Scanned | Merged release-4.6 backport of the COSEM recursion-depth lifetime fix captured from !25935. Confirms the depth guard must remain active through the recursive child call. |
| !25940 | Scanned | Merged automatic assigned-number/manufacturer/service update for release-4.4. Generated/reference-data maintenance; no reusable engineering lesson. |
| !25941 | Scanned | Merged automatic assigned-number/manufacturer/service update for release-4.6. No additional review lesson. |
| !25942 | Scanned | Merged automatic assigned-number/manufacturer/service update for master. No additional review lesson. |
| !25943 | Discussion-focused | Closed draft exploring a Lua Windows UTF-8 manifest approach. Discussion clarified that manifest placement/behavior differs between Lua used as a Wireshark DLL and the standalone `lua.exe`; because the MR was closed with unresolved packaging/platform questions, no design was promoted as accepted practice. |
| !25944 | Discussion-focused | Open draft exploring Windows 11 24H2+ detached console allocation for Wireshark/Stratoshark while preserving ordinary console behavior for CLI tools such as dumpcap. The compatibility behavior on Windows 10 remained a design issue, so the proposal is retained only as contextual evidence. |
| !25945 | Scanned | Merged IEEE 802.11 security extension. Alexis La Goutte explicitly requested a pcap; the contributor supplied `8021x_auth.pcap` before merge. Corroborates the existing notebook convention that materially changed dissectors should include representative captures. |
| !25946 | Deep | Merged NMEA0183 consolidation. Pascal Quantin requested reuse of the already fetched sentence ID and pointed to existing `epan/strutil.h` conversion helpers rather than local reinvention. This durable rule was already captured in `dissector-conventions.md` and `review-patterns.md`; no duplicate entry added. |
| !25947 | Deep | Merged John Thacker API-documentation correction distinguishes borrowed packet-pool-lifetime data from newly allocated data that callers must `g_free()`. Promoted the rule that public API documentation must state ownership, lifetime, and release requirements to `parser-api-conventions.md`. |
| !25948 | Scanned | Closed spelling-only UBX MR with a failed pipeline; superseded immediately by !25949 carrying the same one-line correction. Down-weighted as abandoned/superseded evidence. |
| !25949 | Scanned | Merged successor to !25948 correcting the UBX “Antenna Status” label. Straightforward typo fix; no reusable convention. |
| !25950 | Scanned | Merged John Thacker Export Packet Dissections/C Arrays enhancement adding optional index output and optional omission of secondary data sources while retaining prior defaults. Useful feature work but no sufficiently distinct new general convention extracted. |
| !25951 | Scanned | Merged Couchbase Fusion opcode dissection, including explicit extras/key/value legality and required-value handling for the new commands. Protocol-specific expansion with no substantive review discussion; no new general rule. |
| !25952 | Deep | Merged warning-cleanup MR where Martin Mathieson explicitly sought ETW domain-expert review before accepting a mask/enum change; the expert explained that documented values did not map mechanically to values actually present in the structure and that the proposed semantic change was wrong. Promoted a rule that warning-driven protocol-semantic fixes require specification/runtime validation and, when unclear, domain-expert review. |
| !25953 | Scanned | Merged DECT NR change exposing a public context structure so a parent dissector can supply PHF type. Clean explicit parent-to-child context plumbing, but the MR had no substantive review discussion and was too context-specific to promote separately. |
| !25954 | Deep | Merged John Thacker KNX/IP security fix corrects `tvb_get_ptr()` usage: a length already relative to `offset` must be passed as that byte count, not have the offset subtracted again. Promoted tvbuff offset/length semantics to `parser-api-conventions.md`; !25957 corroborates via release backport. |
| !25955 | Deep | Merged John Thacker CMS hardening clears `algorithm_id` before entering AlgorithmIdentifier parsing so exceptions, empty sequences, or omitted data cannot leave stale state, and handles a missing digest algorithm as unverifiable rather than dereferencing NULL. Promoted reset-before-risky-parse guidance to `parser-api-conventions.md`. |
| !25956 | Deep | Merged John Thacker RTPDump wiretap fix restores the semantic distinction between captured length (`caplen`) and original packet length (`len`), and validates subtraction of the mandatory header with checked arithmetic and `WTAP_ERR_BAD_FILE`. Promoted the wiretap length-metadata rule to `parser-api-conventions.md`. |
| !25957 | Scanned | Merged release-4.6 backport of !25954's KNX/IP `tvb_get_ptr()` correction. Corroborates the promoted offset/length API rule without additional discussion. |
