# Review findings: Wireshark MRs !6311–!6360

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged MRs are treated as accepted evidence unless later follow-up demonstrated a regression or replacement. Closed/draft MRs are lower-weight evidence and are called out explicitly.

| MR | Outcome | Review finding |
|---:|---|---|
| !6360 | merged | Guy Harris release-3.4 backport of the timestamp-validity fix. `frame_data::has_ts` gates timestamp formatting; callers that can legitimately lack timestamps should return an empty column value, while lower-level formatting helpers may assert the precondition. |
| !6359 | merged | BACnet revision-24 update. Alexis La Goutte caught whitespace and guided the contributor toward rebasing and force-pushing the same MR rather than opening a replacement. Useful workflow evidence; little new architectural guidance. |
| !6358 | closed | Earlier release-3.4 timestamp-validity backport using `ws_assert`; Guy Harris replaced it with !6360 because this branch requires `g_assert`. Superseded. |
| !6357 | merged | Master/release timestamp-validity fix adding `has_ts` assertions and an early empty-column path. Corroborates !6360. |
| !6356 | closed | Dumpcap packet-count expansion proposal. John Thacker highlighted the mismatch between dumpcap and Wiretap plugin registration, and later discussion concluded the implementation had been overtaken. Lower-weight architecture evidence only. |
| !6355 | merged | Original timestamp-validity fix; Stig Bjørlykke asked that both timestamp-formatting helpers receive the same validity assertion. Shows paired helper invariants should be applied consistently. |
| !6354 | merged | Adds experimental OQS-OpenSSL PQC OIDs/codepoints. Alexis La Goutte asked whether values were IANA-assigned and pushed source placement/style cleanup. Experimental/private registries should cite their actual authority rather than imply IANA status. |
| !6353 | merged | John Thacker moved HTTP/2 protocol-column/tree creation inside the complete-PDU callback used by `tcp_dissect_pdus`. Avoids an empty HTTP/2 layer on first pass when no full PDU is dissected, preserving first-pass/redissection tree consistency. |
| !6352 | closed | Large SSH/SFTP reassembly draft. John Thacker asked for realistic segmentation captures with GRO/GSO/TSO disabled, separation of unrelated SFTP work, and correct bidirectional SSH channel identity; later closed as superseded. Useful testing/review evidence but not accepted design precedent. |
| !6351 | merged | CIP Forward Close gains connection parameters and links back to Forward Open state. Primarily protocol feature work; no new broad convention extracted. |
| !6350 | merged | CIP Safety refactoring to prepare later protocol updates. Clean preparatory refactor; no novel durable convention beyond keeping behavior-preserving groundwork separate. |
| !6349 | closed | John Thacker TLS draft documents why protocol-layer numbers can differ between first pass and redissection when desegmentation suppresses later calls. He explicitly abandoned the TLS-local workaround in favor of TCP-layer fixes !6567/!6666, which are stronger accepted evidence. |
| !6348 | merged | GlusterFS RPC credential decoding fixes a field-width issue. Reviewer questioned the prose/code mismatch around seconds versus nanoseconds; reinforces that commit/MR descriptions should accurately name the field actually changed. |
| !6347 | merged | TLS avoids overwriting Info text for an MSP that starts and ends in the same frame. The display result should reflect the actual reassembly state, not merely the fact that an MSP object exists. |
| !6346 | merged | User-guide grammar cleanup. No new coding convention. |
| !6345 | merged | Gerald Combs fixes a MySQL metadata index overrun by validating the derived index before array access and reporting an expert error. Strong evidence that count-derived indexes must be bounds-checked even after the backing arrays were sized correctly. |
| !6344 | merged | ORAN FH-CUS preparatory work for modulation compression. Mostly protocol evolution/refactoring; no broad new rule extracted. |
| !6343 | merged | WSLua `TreeItem:add_packet_field` is made consistent across many field types by using native `proto_tree_add_item_ret_*` helpers, returning both decoded value and next offset, documenting the contract, and adding tests. Roland Knall raised compatibility concerns; the accepted change was appropriate for the upcoming major release. |
| !6342 | merged | MySQL allocation count cast added for the typed array helper. Routine type-correctness follow-up to !6340. |
| !6341 | merged | SCCP XUDT segmentation option handling fix. Protocol-specific parsing correction; no broad new convention. |
| !6340 | merged | Gerald Combs fixes the half-sized MySQL metadata allocation introduced in !6311 by replacing manual byte-size arithmetic with `wmem_alloc0_array(type, count)`, validating untrusted counts before narrowing, and adding an expert diagnostic. Strong count/allocation evidence. |
| !6339 | merged | CBOR exposes header components as fields. Mostly protocol presentation work; no new broad convention extracted. |
| !6338 | merged | NVMe fixes with reviewer-requested indentation cleanup and screenshots. Useful evidence that UI-visible dissector changes can benefit from concrete before/after validation, but no deeper convention. |
| !6337 | merged | Guy Harris PacketLogger SCO support. Reverse-engineered packet types are accepted when the format has no public specification, but the lack of a normative source should be explicit. |
| !6336 | merged | Guy Harris stable-branch PacketLogger SCO support, corroborating !6337. |
| !6335 | merged | Guy Harris cleans PacketLogger by centralizing HCI dispatch and direction setup in one helper. Consolidate duplicated dispatch/context setup when adding adjacent packet types. |
| !6334 | merged | João Valverde adds explicit display-filter syntax to disambiguate field names from literals. Durable principle: ambiguous lexical domains should have explicit user syntax instead of relying solely on registry lookup precedence; later dfilter MRs refine the current syntax/semantics. |
| !6333 | merged | Stig Bjørlykke PacketLogger SCO support. Alexis asked for a spec; Guy Harris explicitly noted the work came from reverse engineering. Corroborates !6337 but carries less authority than Guy's versions. |
| !6332 | merged | `tshark -G plugins` initializes codec plugins so they appear in output. Introspection/reporting modes must initialize every subsystem whose registrations they promise to enumerate. |
| !6331 | merged | John Thacker prevents MP2T subdissector calls on non-final fragments. Frame number plus layer number can still be ambiguous when multiple transport-stream packets share a frame/layer; use protocol-local completion state before dispatching the reassembled payload. |
| !6330 | merged | John Thacker makes TCP reassembly check both the frame number and protocol-layer number before treating the current occurrence as the final segment. Frame number alone is not sufficient identity when the same transport protocol appears multiple times in one frame. |
| !6329 | merged, later regressed | Nested dependent-frame support moved dependency lists onto `frame_data`, but post-merge GUI testing exposed a crash. Later fixes/reverts reviewed in higher-numbered MRs outweigh this implementation. Important negative evidence: merged lifecycle changes still need GUI and filtered-save regression testing, not only tshark. |
| !6328 | merged | Adds display-filter semantic-checker debug logging. Diagnostic instrumentation should preserve source context and be compiled away cleanly when debugging is disabled. |
| !6327 | merged | Corrects display-filter VM dump operator spellings. Debug/introspection output should use the same semantic operator vocabulary users see, or it becomes misleading during diagnosis. |
| !6326 | merged | macOS dSYM packaging split into a dedicated image/package with version-specific installation instructions. Packaging artifacts that are only useful for matching binaries should make version coupling explicit. |
| !6325 | merged | Automated data/release-note refresh. No new convention. |
| !6324 | merged | Automated PCI/vendor data refresh. No new convention. |
| !6323 | merged | Automated registry/translation refresh. No new convention. |
| !6322 | merged | John Thacker converts LI5G payload dispatch to a dissector table. Extensible protocol subtype dispatch belongs in registration tables rather than fixed private handle arrays when new payload formats may be added independently. |
| !6321 | merged | LI5G uses `eth_maybefcs` rather than a nonexistent/incorrect Ethernet dissector handle. Choose the semantic dissector entry point that matches uncertainty about trailing FCS. |
| !6320 | merged | Manpage wording cleanup with Jaap Keuter correcting over-broad edits. Documentation cleanups should preserve deliberate wording that describes actual tool-specific behavior. |
| !6319 | merged | Packet Details context-menu documentation clarification. No coding convention. |
| !6318 | merged | LI5G converts raw string arrays/constants to `value_string` tables and shared IP protocol values. Prefer Wireshark's standard value-string/registry representations over ad hoc local lookup arrays. |
| !6317 | merged | LI5G spelling cleanup. No new convention. |
| !6316 | merged | TShark removes exclamation marks from routine overwrite diagnostics. User-facing CLI diagnostics should be factual and calm rather than unnecessarily alarming. |
| !6315 | merged | LI5G indentation cleanup. No new convention. |
| !6314 | merged | LI5G adds UDP registration parallel to TCP. Routine protocol registration expansion. |
| !6313 | merged | Adds MPEG FTA Content Management Descriptor fields with masks. Straightforward descriptor support; no broad new convention beyond explicit bit-field masks. |
| !6312 | merged | macOS notarization command update from obsolete `--eval-info` to `--notarization-info`. External tool integrations should use the tool's current command vocabulary and print the exact failing command for diagnosis. |
| !6311 | merged, later fixed | Large MariaDB/MySQL feature merge. Alexis La Goutte caught field encoding/type misuse and static-analysis dead stores, but Gerald Combs later identified a half-sized metadata allocation and a separate buffer overflow fixed by !6340 and !6345. Treat this as negative evidence that a merged large stateful dissector refactor can still need immediate memory-safety follow-ups. |

## High-authority takeaways

The strongest accepted evidence in this batch comes from Guy Harris (!6360, !6337, !6336, !6335), John Thacker (!6353, !6331, !6330), João Valverde (!6334), and Gerald Combs (!6340, !6345). Closed !6349 is valuable mainly because John explicitly explains why he abandoned the local TLS workaround in favor of the later TCP architecture already captured elsewhere in the notebook.
