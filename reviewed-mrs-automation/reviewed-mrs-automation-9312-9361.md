# Wireshark MR review ledger — !9361 through !9312

Reviewed 2026-10-31. Corpus: `dheitmueller/wireshark-corpus-mrs` at exact commit `ddcaa22b51c68f594e425a23388c3a2086813054`. Notebook starting head: `b7289d7b48113b03cbb1b65db3e97d66880650a2`.

**Exact reviewed membership: 50 distinct MRs in descending order, !9361 through !9312 inclusive; 44 merged and 6 closed without merging (!9344, !9334, !9330, !9325, !9323, !9316).** !9361, !9360, !9359, !9358, !9357, !9356, !9355, !9354, !9353, !9352, !9351, !9350, !9349, !9348, !9347, !9346, !9345, !9344, !9343, !9342, !9341, !9340, !9339, !9338, !9337, !9336, !9335, !9334, !9333, !9332, !9331, !9330, !9329, !9328, !9327, !9326, !9325, !9324, !9323, !9322, !9321, !9320, !9319, !9318, !9317, !9316, !9315, !9314, !9313, !9312.

## Selection and duplicate exclusion

Inventoried 359 existing per-run/supplemental files in `reviewed-mrs-automation/` and separately inspected the root `reviewed-mrs.md` and supplemental `reviewed-mrs-automation.md`. Inspected per-run explicit membership rather than interpreting filename intervals as proof, including several exceptional ledgers: !10963–!11012 has a two-line placeholder but its sibling `reviewed-mrs-automation-10963-11012-exact-list.tmp` enumerates all 50; !10463–!10512 uses an MR-prefixed list; !19940–!19989 uses a plain numeric list; !15376 was explicitly unreviewable/omitted due to empty corpus record; other noncontiguous reviewed ranges include !15899–!16048, !17559–!17658, !22943–!22993, and !26317–!26412. Root `reviewed-mrs.md` explicitly supplies the already-reviewed !26365–!26393 members. Cross-checked the adjacent !9362–!9411 ledger, which contains 50 distinct entries. None of the candidate !9361–!9312 MR numbers occurs in the previous review tracking. All 50 candidate JSON records exist in the pinned corpus. The historical `reviewed-mrs-automation-17571-17620.md` has exactly 50 distinct explicit table entries and remains unchanged.

## Exact 50-MR assessments

| MR | Status | Review depth | Evidence / durable assessment |
| --- | --- | --- | --- |
| !9361 | merged | Deep | João Valverde moves Flex debugging to generated scanner build options and keeps runtime trace controls conditional on non-NDEBUG builds. |
| !9360 | merged | Scanned | Gerald Combs corrects AsciiDoc markup in capture-file FAQ paths. |
| !9359 | merged | Deep | HTTP/2 and CBOR allocate composite TVBs only on first appended member; avoid finalizing an empty composite and test zero-length indefinite CBOR. |
| !9358 | merged | Scanned | Guy Harris updates macOS bootstrap Autoconf/Automake versions needed by minizip. |
| !9357 | merged | Scanned | ALP adds A/330 Link Mapping Table and documented third-party Sony extensions with example pcaps. |
| !9356 | merged | Scanned | dftest adds no-optimization syntax-check option and improves command-line handling. |
| !9355 | merged | Discussion-focused | New ATSC LLS dissector: Alexis requests Clang dead-store cleanup and release notes; Chuck Craft identifies MSVC narrowing warning, repaired in !9375. |
| !9354 | merged | Scanned | Automated registry, generated help, and translation update. |
| !9353 | merged | Scanned | Automated PCI and manufacturer registry update. |
| !9352 | merged | Scanned | Automated registry, enterprise IDs, and contributor data refresh. |
| !9351 | merged | Scanned | USBLL implements USB 2.0 LPM extension and corrects the BESL terminology following errata. |
| !9350 | merged | Scanned | DFVM debug dump includes filter-field layer/occurrence data. |
| !9349 | merged | Deep | John Thacker adds cancelable packet-list sorting synchronized with capture read lock and re-sort after rescan and column invalidation. |
| !9348 | merged | Scanned | Zigbee ZDP sanitizes packet-originated language bytes before string presentation. |
| !9347 | merged | Scanned | DFVM string representation omits trailing newline for logging; file dump writes its own newline. |
| !9346 | merged | Scanned | Display-filter test families consolidated and syntax-test coverage expanded. |
| !9345 | merged | Scanned | Display-filter VM dump and dftest logging/output improvements. |
| !9344 | closed | Discussion-focused | Unmerged unique exit-code draft: Martin Mathieson seeks a distinguishable invalid-filter exit; João supports concept; contributor closes to resubmit cleanly. |
| !9343 | merged | Deep | John Thacker removes redundant Qt packet-list reset after display-filter rescans because captureFileReadFinished already invalidates columns. |
| !9342 | merged | Scanned | Correct WSLua method-specific argument-name macros for ByteArray LE getters. |
| !9341 | merged | Scanned | Correct Bluetooth LE feature masks according to updated 5.3 feature definitions. |
| !9340 | merged | Scanned | Stable branch GNW packet data stores security TVB in pinfo pool rather than file scope; counterpart of !9322. |
| !9339 | merged | Scanned | Stable branch GNW packet data scope fix; counterpart of !9322. |
| !9338 | merged | Scanned | Stable branch Q.850 wording correction to Interworking. |
| !9337 | merged | Scanned | Stable branch Q.850 wording correction to Interworking. |
| !9336 | merged | Deep | Martin Mathieson reverts unsafe Boolean field conversions: unmasked 0/1 values require numeric FT_UINT8 with value strings rather than FT_BOOLEAN that treats every nonzero as true. |
| !9335 | merged | Discussion-focused | Alexis reports Clang Analyzer dead stores in Locamation parser, prompting cleanup beyond initial Coverity warning. |
| !9334 | closed | Discussion-focused | PFCP monolithic IE refactor: Alexis requests analyzer/whitespace fixes and dissector table design; Anders asks to split large change and isolate fixes; accepted subset follows in !9363. |
| !9333 | merged | Scanned | DFVM dump stack cleanup handles absent stack and bounded stack popping. |
| !9332 | merged | Scanned | dftest usage and diagnostic output reshaping. |
| !9331 | merged | Scanned | IPv4/IPv6 display-filter value representation preserves non-default CIDR prefix. |
| !9330 | closed | Discussion-focused | HTTP/3 prototype mixes unrelated signing/packaging; Alexis requests dedicated work and small pcap, while Gerald permits separate logical commits for employer legal review. |
| !9329 | merged | Scanned | Mainline Q.850 wording correction to Interworking; stable siblings !9337 and !9338. |
| !9328 | merged | Scanned | dftest CLI and DFVM output refactor makes dump function take caller FILE pointer. |
| !9327 | merged | Deep | Gerald Combs replaces assertion with FragmentBoundsError when TCP one-more-segment reassembly cannot occur for external reasons. |
| !9326 | merged | Scanned | ACN adds sACN Universe Sync protocol handling. |
| !9325 | closed | Scanned | Unmerged sACN Universe Discovery draft; do not treat code as accepted. |
| !9324 | merged | Scanned | DNS record-type numbers updated from IANA definitions. |
| !9323 | closed | Scanned | Unmerged draft fixing Lua warnings; use accepted !9321 instead. |
| !9322 | merged | Deep | Gerald Combs stores GNW per-packet security child TVB through p_add_proto_data(pinfo->pool), not file-scope allocation. |
| !9321 | merged | Deep | Gerald Combs corrects MSVC warning-as-error for guint64 to lua_Number argument in WSLua after separate closed draft. |
| !9320 | merged | Scanned | Stable branch Guy Harris GSList data-source fix: use source->data instead of casting list node. |
| !9319 | merged | Discussion-focused | O-RAN usability tweaks; Martin Mathieson flags unrelated Windows CI failure in WSLua code. |
| !9318 | merged | Scanned | Stable branch Guy Harris GSList source->data fix. |
| !9317 | merged | Discussion-focused | Androiddump adapts Bluetooth process discovery to Android T APEX; Alexis checks older-version commands remain correct. |
| !9316 | closed | Discussion-focused | Unmerged permissive GSM PCO zero-length patch; Pascal Quantin advises explicit standards-nonconformance diagnostic and systematic PCO validation. |
| !9315 | merged | Deep | ODVA CIP deprecated/new TLV IDs; shared parsing helper accepts explicit encoding and reviewers distinguish draft from published spec. |
| !9314 | merged | Scanned | Mainline Anders Broman GSList source->data fix; stable siblings !9318 and !9320. |
| !9313 | merged | Scanned | NAS EPS decoder updates to 3GPP TS 24.301 v17.9.0. |
| !9312 | merged | Scanned | OPC UA DiagnosticInfo fixes wire order: Locale precedes LocalizedText. |

## Promoted rules and evidence hierarchy

**High-confidence merged implementation evidence:** composite TVB ownership and empty-CBOR regression (!9359); packet-scope child-TVBuff proto data (!9322 with backports !9339 and !9340); whole-value numeric field semantics (!9336); recoverable TCP reassembly exception versus programmer assertion (!9327); capture-safe cancelable sort and single column-invalidation lifecycle (!9349, !9343); GSList node-versus-data pointer correctness (!9314, backports !9318/!9320); release-build scanner tracing (!9361).

**Authoritative review feedback:** Alexis La Goutte requests new-dissector release notes, Clang Analyzer cleanup and Windows compiler fixes (!9355); Alexis and Anders Broman direct the oversized PFCP proposal into split, reviewable pieces with separate bug fixes and optional dissector-table dispatch (!9334, closed); Alexis and Gerald Combs distinguish logically separate commits from a contributor's employer-mandated single legal approval for unrelated HTTP/3 and packaging work (!9330, closed); Pascal Quantin argues nonconforming zero-length GSM PCO should produce expert diagnosis, not be silently treated as valid (!9316, closed). Closed implementations are **not** accepted implementation precedents; the reviewer principles are credited appropriately.

**Limits:** All 50 source JSON metadata/description snapshots, available discussion notes and file-level diffs were inspected; some large binary/diff records are unavailable or empty in corpus (notably the unmerged HTTP/3 draft). No build, CI, runtime dissector execution, or fuzzing was performed. Status refers to pinned corpus snapshot, not subsequent live GitLab history. Existing notebook guidance takes precedence where newer/stronger corrective work exists.