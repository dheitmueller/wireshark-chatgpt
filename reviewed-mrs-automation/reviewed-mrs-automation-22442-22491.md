# Wireshark MR review automation ledger: !22442–!22491

- **Corpus:** `dheitmueller/wireshark-corpus-mrs`
- **Corpus commit:** `9e52bc78659a888d4eb624984ee1a886a40d959f`
- **Review direction:** descending from newest available toward older MRs.
- **Selection method:** rebuilt the already-reviewed set from the available files under `reviewed-mrs-automation/` plus `reviewed-mrs.md`, explicitly including the historical !17571–!17620 batch; then selected the 50 highest-numbered corpus MRs absent from that set. Candidate files were checked against the corpus rather than inferring reviewed/unreviewed status solely from numeric ranges.
- **Exact reviewed set (50):** !22491, !22490, !22489, !22488, !22487, !22486, !22485, !22484, !22483, !22482, !22481, !22480, !22479, !22478, !22477, !22476, !22475, !22474, !22473, !22472, !22471, !22470, !22469, !22468, !22467, !22466, !22465, !22464, !22463, !22462, !22461, !22460, !22459, !22458, !22457, !22456, !22455, !22454, !22453, !22452, !22451, !22450, !22449, !22448, !22447, !22446, !22445, !22444, !22443, !22442.

## Per-MR review notes

| MR | Classification | Review result |
|---:|---|---|
| !22491 | Scanned | Merged release-4.6 CI backport enabling release tests in Windows package jobs. Useful validation coverage but no new convention beyond existing CI/test guidance. |
| !22490 | Corroboration | Guy Harris-authored/merged Cosem backport removes unsigned post-decrement loop underflow flagged by Coverity. Reinforces existing arithmetic-underflow and semantic-type guidance. |
| !22489 | Scanned | Merged Zigbee ZCL Generic Tunnel support. Protocol-specific feature; no distinct durable review rule found. |
| !22488 | Corroboration | Guy Harris-authored/merged OSS-Fuzz Cosem fix changes a count returned as unsigned from signed `int` to `unsigned`, avoiding signed-overflow UB. Reinforces arithmetic/type-domain guidance. |
| !22487 | Scanned | Falco dependency/plugin data update. Build/dependency maintenance, no new durable rule. |
| !22486 | Scanned | Revert of Windows known-folder change after discovering it required a desktop session. Useful platform-compatibility evidence but no additive notebook convention. |
| !22485 | Deep / corroboration | Large merged Lua Debugger feature underwent prolonged hands-on crash, UX, security, race and lifecycle review before merge. John Thacker also caught unrelated downloaded-library files accidentally included from an in-source `WIRESHARK_BASE_DIR`; reinforces clean-diff/submission-scope discipline. |
| !22484 | Corroboration | Merged `-Wpointer-sign` cleanup. Reinforces matching pointer signedness to byte/string semantics and API contracts. |
| !22483 | Scanned | Merged ISAKMP Unity attribute dissection extension. Protocol-specific functionality without broader review guidance. |
| !22482 | Scanned | Merged O-RAN tap display decomposition of eAxC subfields. Presentation/tap-specific enhancement. |
| !22481 | Scanned | Merged NMEA0183 UDP broadcast handling. Protocol-specific dispatch correction; no new durable rule. |
| !22480 | Corroboration | Merged `-Wpointer-sign` cleanup; no distinct new convention. |
| !22479 | Corroboration | Windows `-Wpointer-sign` fixes reinforce matching Windows/API text and byte pointer types to actual contracts. |
| !22478 | Corroboration | Merged `-Wpointer-sign` cleanup; no distinct new convention. |
| !22477 | Corroboration | Merged one-line `-Wpointer-sign` fixes; no distinct new convention. |
| !22476 | Corroboration | BMP TLV parsing/offset and expert-info corrections reinforce exact parser-coordinate accounting and malformed-region diagnostics. |
| !22475 | Deep / corroboration | John Thacker-authored merged Riemann cleanup replaces hand-written protobuf/zigzag varint decoding with `tvb_get_varint()`. Strong helper-reuse evidence; existing helper/API guidance already covers it. |
| !22474 | Corroboration | Merged pointer-sign warning fixes; no new convention. |
| !22473 | Scanned | Merged Galileo OSNMA MACK reassembly/dissection feature. Reassembly-specific implementation without a broader new convention in this review. |
| !22472 | Scanned | Windows package CI prints the produced TShark version. Diagnostic CI improvement; no new durable rule. |
| !22471 | Deep / corroboration | Merged master Cosem fix replaces packet-triggerable `DISSECTOR_ASSERT_HINT()` paths with descriptive expert information and uses a return-value tree helper to avoid duplicate fetching. Strongly corroborates existing malformed-input/assertion and helper-use rules. |
| !22470 | Down-weighted | Closed draft DIAMETER grouped-AVP presentation experiment. Michael Mann requested sample evidence and objected to an extra dynamic-formatting layer, suggesting normal tree relationships/`proto_item_append_text()` instead. Later accepted work is stronger evidence; this abandoned design is not an exemplar. |
| !22469 | Scanned | Adds protocol information for AIM/alljoyn. Metadata/documentation-style maintenance. |
| !22468 | Corroboration | Merged dissector pointer-sign fixes; reinforces byte-array versus text-pointer semantics. |
| !22467 | Deep / promoted | John Thacker-authored/merged master fix moves extremely weak Ixia VeriWave and CAM Inspector wiretap heuristics to the end because they falsely claimed valid JSON. Promoted to `wiretap-file-detection-conventions.md`: recognizer order is behavioral and weak heuristics must not preempt stronger/specific formats. |
| !22466 | Scanned | Documents `mark_frame_as_depended_upon()`. Useful API documentation, no distinct architectural rule beyond existing dependency/redissection guidance. |
| !22465 | Down-weighted / discussion-focused | Open snapshot proposing hidden raw UTC fields for automated filtering. John Thacker pointed to raw output modes and the display-filter `@` operator as alternatives, with endian caveats. Because unresolved/unmerged, treated as provisional negative guidance only. |
| !22464 | Scanned | DECT NR SNR/sequence rollover handling variant/backport. Protocol-specific state correction. |
| !22463 | Scanned | DECT NR SNR/sequence rollover handling. Protocol-specific state correction; no general rule promoted. |
| !22462 | Corroboration | Continued TVBuff subset conversion eliminates `-1` captured/reported-length sentinel use. Reinforces explicit bounded/remaining-length TVBuff APIs already recorded. |
| !22461 | Scanned | WSDG restores missing Proto functions. Documentation repair, no new coding convention. |
| !22460 | Scanned | PeekTagged `cpid` section handling. File-format-specific parsing support; no distinct general rule. |
| !22459 | Scanned | ISOBUS Time/Date year decoding fix/backport. Protocol-specific correctness fix. |
| !22458 | Scanned | Disabled-protocol list simplification. Preference/state cleanup with no new durable rule. |
| !22457 | Corroboration | PER bit-length calculation fixes include the final partially used byte and handle zero bits. Reinforces exact encoded-range accounting. |
| !22456 | Deep / corroboration | John Thacker-authored/merged HTTP/3 backport caps decompressed QPACK header output at 1 MiB and reports an expert item, explicitly defending against decompression bombs. Strong corroboration of existing input-resource-limit guidance. |
| !22455 | Scanned | PeekTagged `cpid` handling variant. File-format-specific parsing support. |
| !22454 | Corroboration | MSYS2/Windows pointer-sign build fix aligns `utf_16to8()` output and `strlen()` with `char *`; reinforces semantic pointer typing. |
| !22453 | Deep / promoted | John Thacker-authored, Michael Mann-merged ASTERIX preference fix notes C17 enum signedness is implementation-defined while the preference API stores through `int *`. Promoted to `c-type-conventions.md`: backing storage must match the API pointer contract, not an assumed enum representation. |
| !22452 | Deep / corroboration | Martin Mathieson-authored/merged O-RAN change adds timing identity to an insufficiently unique reassembly key. Reinforces existing rule that reassembly identity must contain enough stable discriminators to prevent unrelated fragments colliding. |
| !22451 | Scanned | Merged RTCP MCPC support. Protocol-specific feature without broader durable review feedback. |
| !22450 | Corroboration | Stable LIN subdissector-call fix. Reinforces that subdissector invocation is semantic behavior and must not be accidentally suppressed. |
| !22449 | Scanned | Zigbee Touchlink transaction-ID double-parsing correction. Protocol-specific parser ownership fix. |
| !22448 | Scanned / superseded context | Earlier Zigbee Touchlink transaction-ID correction variant. Later !22449 is the cleaner accepted context; no separate convention extracted. |
| !22447 | Scanned | Distinguishes separate I/O graph and plot graph `value_string` tables to avoid semantic/name confusion. Local type/data hygiene. |
| !22446 | Deep / corroboration | Merged ICMPv6 DNR option dissector includes an explicit sample pcap and demonstrated output. Reinforces existing sample-capture expectation for protocol additions; no new rule needed. |
| !22445 | Corroboration | RTPDump time handling uses the existing encoded-time support directly rather than manually reconstructing it. Reinforces semantic helper reuse. |
| !22444 | Corroboration | Guy Harris-style type/representation cleanup treats RTPDump magic as an octet array rather than pretending it is a NUL-terminated C string. Reinforces wire-representation-versus-C-representation discipline. |
| !22443 | Corroboration | Corrects CMake classification of `packet-log3gpp` as ordinary rather than ASN.1-generated source. Reinforces generated-source ownership/inventory accuracy. |
| !22442 | Corroboration | ASN.1 cleanup keeps a printable ASCII temporary as `char *` rather than converting between byte and character pointer types. Reinforces semantic pointer typing. |

## Notebook changes from this batch

1. Added `wiretap-file-detection-conventions.md` from merged master !22467: file-recognizer order is correctness behavior; strong/specific recognizers should precede weak heuristics that can falsely claim unrelated files.
2. Extended `c-type-conventions.md` from merged master !22453: preference/API backing storage must match the API's declared pointer type; do not rely on implementation-defined C17 enum signedness as a storage contract.
3. Kept !22471, !22456, !22452, !22475 and the pointer-sign/TVBuff series as corroboration of already-recorded malformed-input, resource-limit, reassembly-identity, helper-reuse, and API-domain rules rather than duplicating them.
4. Explicitly down-weighted open !22465 and closed draft !22470; their discussion is useful, but neither snapshot is accepted architecture.

The previously reviewed !17571–!17620 historical batch remains part of the already-reviewed set and was preserved/counts for future selection.
