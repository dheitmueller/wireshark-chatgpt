# Wireshark MR review findings — 4561–4610

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Weighting: merged master work is primary evidence; maintained-branch backports corroborate their master origins; closed or superseded proposals are lower-weight history. No substantive Guy Harris discussion comment appeared in this batch, but Guy-authored merged master work is called out where relevant.

| MR | Outcome | Review finding |
|---|---|---|
| !4610 | Merged | 3.6.0rc0 release-build metadata. Low architectural value; confirms release metadata and generated release material move together. |
| !4609 | Merged | Release-3.6 backport adding captype to both NSIS and WiX installers; corroborates the master packaging change. |
| !4608 | Merged | Release-3.4 backport of the HCI ISO reassembly destination-capacity fix. |
| !4607 | Merged | Release-3.6 backport of the HCI ISO reassembly destination-capacity fix. |
| !4606 | Merged | Adds Osmocom-specific RSL IEs and a preference controlling those definitions; no substantive review discussion beyond accepted implementation. |
| !4605 | Merged | Adds captype executable and documentation consistently to NSIS, WiX, and uninstall packaging. New shipped tools must be represented across all supported installer surfaces. |
| !4604 | Merged | Bounds Bluetooth SDP continuation state to the protocol maximum before retaining it, reports malformed excess length, and preserves safe partial handling. |
| !4603 | Merged | Gerald Combs bounds each HCI ISO fragment copy by remaining reassembly-buffer capacity and reports excess data instead of overrunning the destination. |
| !4602 | Merged | Release-3.6 backport of the eNode-B file-probe ownership fix. |
| !4601 | Merged | Fixes eNode-B file recognition so input without the required magic returns NOT_MINE rather than being claimed. |
| !4600 | Merged | Jaap Keuter caught a missing value_string terminator; accepted code reinforces the required sentinel-terminated table contract. |
| !4599 | Merged | WebSocket fragmented messages gain stable per-conversation reassembly identity and preserve first-fragment opcode until the completed payload can be dissected. |
| !4598 | Merged | Adds current Couchbase subdocument error codes; clean successor to the abandoned predecessor. |
| !4597 | Closed | Superseded Couchbase draft; implementation precedent comes from the merged successor, not this draft. |
| !4596 | Merged | Updates GPRS charging ASN.1 to 3GPP TS 32.298 V17.0.0 and regenerated dissector output; primarily specification maintenance. |
| !4595 | Merged | Refines TCP ACK-unseen bookkeeping so one missing segment does not cause a run of duplicate warnings on contiguous pure ACKs. |
| !4594 | Merged | Improves SIP 2xx response Info-column method context; narrow presentation change. |
| !4593 | Merged | Corrects extcap startup diagnostics that accidentally referred to captype; user-facing diagnostics should identify the actual component. |
| !4592 | Merged | John Thacker models uTP conversations with protocol connection IDs, handles direction-dependent paired IDs and partial/wildcard discovery, and assigns a stable generated stream ID. |
| !4591 | Merged | Fuzzing exposed nullable/partially initialized BP state; accepted fixes guard optional parsed pointers before dereference or map insertion. |
| !4590 | Merged | Clang Analyzer cleanup removes genuine dead stores and corrects a dissector return to the actual parsed offset; warning fixes were kept semantically narrow. |
| !4589 | Merged | Release-3.6 backport of Asciidoctor-native CSS copying. |
| !4588 | Closed | Abandoned release-3.6 initialization attempt; broad branch-generation history only, not implementation precedent. |
| !4587 | Merged | Stable backport of the BT-DHT parser-progress guard from the master fix. |
| !4586 | Merged | Corrects Bluetooth LE SMP debug public-key byte order to match little-endian wire semantics. |
| !4585 | Merged | Capinfos documentation/usage corrections, including long option presentation and RIPEMD naming. |
| !4584 | Merged | Anders Broman caught copy/paste errors in PFCP flag masks during a large specification update; repeated bitmask blocks deserve explicit per-bit verification. |
| !4583 | Closed | First WebSocket reassembly attempt was closed because fragment identity was not yet sound; useful negative evidence for requiring a complete reassembly key before merging. |
| !4582 | Merged | Release-3.6 backport of the TCP Follow explicit-ACK-validity fix. |
| !4581 | Closed | Superseded Couchbase submission. Alexis La Goutte asked the contributor to rebase and not use the long-lived master branch; the clean successor merged. |
| !4580 | Merged | check_static cleanup removes an obsolete BPv7 helper after the component author confirms it was a leftover from pre-wmem manual memory management. |
| !4579 | Merged | Release-3.6 backport of the non-exported g_memdup2 compatibility shim. |
| !4578 | Closed | Draft uTP Follow Stream work contains useful rationale about protocol-level stream identity but remained unmerged; down-weight architecture proposals from it. |
| !4577 | Merged | TCPCLv4 keeps wire-negotiated versions in one protocol implementation, adds captures/tests/fuzzing, and after Alexis review splits unrelated bug fixes into a separate MR. |
| !4576 | Merged | John Thacker prevents placeholder ACK value 0 from entering wrap-aware sequence arithmetic by carrying explicit ACK-validity state. |
| !4575 | Merged | Adjusts display-filter byte conversion to replacement semantics; internal semantic cleanup with no substantive discussion. |
| !4574 | Merged | Guy Harris-authored release-3.6 backport removing unused AUTOSAR NM protocol-ID lookups. |
| !4573 | Merged | Uses Asciidoctor's own copycss attribute rather than separate CMake copy plumbing for release notes and FAQ. |
| !4572 | Merged | Splits display-filter syntax-node stringification into distinct formats; internal parser/debug representation refactor. |
| !4571 | Merged | Guy Harris-authored master cleanup removes protocol-ID lookups whose values are never used, avoiding unnecessary registry coupling/work. |
| !4570 | Merged | Gerald Combs enforces a generic parser progress invariant: after each bencoded-list element, terminate with expert info if the offset failed to increase. |
| !4569 | Merged | Keeps the pre-GLib-2.68 g_memdup2 compatibility implementation static-inline so wsutil does not export a symbol owned by another library's namespace. |
| !4568 | Merged | Debian packaging refresh for 3.6; accepted release packaging maintenance. |
| !4567 | Merged | Protocol filter names are rejected at registration if they collide with display-filter reserved keywords, preventing parser/registry ambiguity. |
| !4566 | Closed | Proposed Lua Field.new() behavior change was closed unmerged; do not treat the nil-on-missing API behavior as accepted precedent. |
| !4565 | Merged | Continues Qt migration to type-safe new-style signal/slot connections; mechanical modernization. |
| !4564 | Merged | macOS exposed that 64-bit width alone does not choose a safe printf macro: GLib integer typedefs and the actual formatting stack must agree. Pascal Quantin and João Valverde supplied the key review nuance. |
| !4563 | Closed | Display-filter syntax-tree reference-counting optimization was abandoned by its author as probably not worth the complexity. |
| !4562 | Merged | Fixes Bluetooth Mesh compilation without GCRYPT; optional-feature guards must cover every code path that references the dependency. |
| !4561 | Merged | Adds AUTOSAR I-PduM and integrates it through existing dissector tables/context structures into NM, ISO15765, and Signal PDU consumers. |
