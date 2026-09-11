# Automated Wireshark MR review: !23059-!23108

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Review direction: newest to oldest.

This run reviewed exactly the 50 highest-numbered corpus MRs not already represented by the review tracking available before the run. Selection was made from individual MR entries across `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, and the per-run files in `reviewed-mrs-automation/`; numeric ranges were not assumed reviewed merely from their filenames. The historical !17571-!17620 batch was explicitly preserved and counted as already reviewed.

## Exact reviewed set

| MR | Depth | Disposition / finding |
|---|---|---|
| !23108 | Scanned | Merged release-4.6 backport of !23105. IDN precision-array index fix; no additional lesson. |
| !23107 | Corroboration | Merged. Converts LE 32-bit dissector sites to return-value tree helpers instead of separate fetch/add operations. |
| !23106 | Deep | Merged. Core FT_STRINGZ handling reports unsupported encoding combinations as `DISSECTOR_BUG` rather than silently decoding as ASCII; discussion points toward checker coverage. |
| !23105 | Deep/corroboration | Merged master. IDN loop starts at array index 0 and uses a conventional `<` bound, fixing a buffer overrun. |
| !23104 | Corroboration | Merged. Rewrites a remaining-length parity check so it is valid even when TVB remaining-length overflow semantics produce zero. |
| !23103 | Deep | Merged. Preferences gain explicit memory-scope ownership instead of hard-coding EPAN/global allocation, improving reuse outside EPAN. |
| !23102 | Deep | Merged. Adds `wmem_tree_get_data_scope()` so users can derive ownership scope from the container instead of assuming a global scope. |
| !23101 | Corroboration | Merged. Continues `expert_remaining`, unsigned-offset, and proto-tree return-value-helper cleanup. |
| !23100 | Corroboration | Merged. Replaces a signed `-1` search-sentinel pattern invalidated by unsigned offsets with a remaining-aware TVB search API. |
| !23099 | Corroboration | Merged. Makes `tvb_new_real_data` reported length unsigned, matching the nonnegative API domain. |
| !23098 | Deep | Merged. Repairs `check_common` so detected source-check errors actually make the checker fail; also fixes recent checker findings. |
| !23097 | Corroboration | Merged. Kafka signed fields use `proto_tree_add_ret_int()` rather than fetch/add duplication. |
| !23096 | Scanned | Merged. SGP.32 reuses the E.212 dissector to expose MCC/MNC from rPLMN. |
| !23095 | Corroboration | Merged. BCD TVB string helpers move nonnegative offsets/lengths to unsigned types. |
| !23094 | Discussion-focused | Merged. Meter 802.11 vendor IE support; Anders Broman requested OUI entries be kept in numerical order and the author adjusted the diff. |
| !23093 | Corroboration | Merged. More `expert_remaining` and unsigned-offset conversion. |
| !23092 | Corroboration | Merged. Coverity annotation/modeling cleanup around a checked GSM RR return path; no new rule beyond existing analyzer guidance. |
| !23091 | Scanned | Merged. Removes/comments out a duplicate SMB TFS value definition. |
| !23090 | Corroboration | Merged. More use of `expert_remaining` helpers. |
| !23089 | Scanned | Merged. S1AP build-portability repair for Ubuntu Plucky. |
| !23088 | Corroboration | Merged. BTS SDP switches to unsigned offsets/lengths and `expert_remaining`. |
| !23087 | Corroboration | Merged. `tvb_bytes_to_str` API moves already-nonnegative offsets/lengths to unsigned types. |
| !23086 | Corroboration | Merged. `tvb_get_bits*` bit counts become unsigned because negative bit counts are outside the API contract. |
| !23085 | Scanned | Merged. Routine December 2025 mailmap update. |
| !23084 | Corroboration | Merged. More `tvb_get_ntohl()` plus tree-add pairs converted to `proto_tree_add_item_ret_uint()`. |
| !23083 | Scanned | Closed/unmerged Ubuntu build-fix attempt; down-weighted. |
| !23082 | Scanned | Merged. Restores a GSM SIM hf bitmask required by `proto_item_add_bitmask_tree()` validation. |
| !23081 | Corroboration | Merged. More `expert_remaining` conversion. |
| !23080 | Scanned | Merged. OER function signature corrected after an earlier constification change exposed a build mismatch. |
| !23079 | Scanned | Merged. WSLua `add_packet_field` gains handling for additional string types, notably variable-length `FT_UINT_STRING`. |
| !23078 | Deep | Merged. Removes `tvb_get_ptr(..., -1)`: a raw pointer must be accompanied by an explicit captured-length bound; most callers already had that length. |
| !23077 | Deep | Merged. Refactors preference APIs away from singleton globals so independent preference trees/files can reuse the subsystem. |
| !23076 | Deep/corroboration | Merged. SOCKS replaces raw-pointer line scanning with `tvb_find_line_end()`, preserving TVBuff bounds semantics. |
| !23075 | Corroboration | Merged. More `expert_remaining` conversion. |
| !23074 | Corroboration | Merged. `tvb_ascii_isprint` family moves to unsigned domains and gains remaining-aware variants. |
| !23073 | Scanned | Merged. Removes a stray source backslash. |
| !23072 | Scanned | Merged. GitHub Actions avoids installing Perl in a build job that does not regenerate dissectors or run Perl tooling. |
| !23071 | Corroboration | Merged. More `expert_remaining` and unsigned-domain cleanup. |
| !23070 | Scanned | Merged automatic data/translation update. |
| !23069 | Scanned | Merged automatic data/translation update. |
| !23068 | Scanned | Merged automatic data/translation update. |
| !23067 | Corroboration | Merged. Adds/adjusts Coverity sanitizer modeling for TVBuff APIs; reinforces analyzer-contract guidance. |
| !23066 | Scanned | Merged. macOS Stratoshark icon packaging update for Tahoe/Liquid Glass. |
| !23065 | Corroboration | Merged. Coverity suppression/annotation recognizes expert modules allocated in EPAN scope are process-lifetime managed. |
| !23064 | Deep/corroboration | Merged. Restores `tvb_ensure_bytes_exist()` in expert-item code even though the triggering OpenFlow dissector also had a bug; preserves a cheap lower-level safety check. |
| !23063 | Scanned | Merged. QCustomPlot compatibility update for Qt 6.9 `QImage` API deprecation. |
| !23062 | Scanned | Merged release-4.4 backport of !23060 H.248 zero-length-TVBuff NULL-deref fix. |
| !23061 | Scanned | Merged release-4.6 backport of !23060 H.248 zero-length-TVBuff NULL-deref fix. |
| !23060 | Deep | Merged master. Treats a non-NULL zero-length BER octet-string TVBuff like the NULL case for initialization, fixing a later NULL dereference. |
| !23059 | Deep | Merged. Bounds each OpenFlow action with a subset TVBuff so malformed zero length raises `ReportedBoundsError` instead of allowing offset regression/infinite looping; closely matches existing bounded-parser/progress guidance. |

## Durable findings promoted

1. **Raw TVBuff pointers require an explicit captured-length bound; prefer TVBuff-native scanning helpers** (!23078, !23076). Added to `tvbuff-parsing-conventions.md`.
2. **Reusable preference APIs should make tree identity and allocator scope explicit rather than assuming singleton/global EPAN state** (!23077, !23102, !23103). Added to new `preference-architecture-conventions.md`.
3. **Unsupported field/encoding combinations supplied by dissector code are programmer bugs, not silent fallback cases** (!23106). Added to `assertion-static-analysis-conventions.md`; Martin Mathieson's suggestion to extend `check_type_item_calls.py` is retained as supporting review evidence.
4. **Repository checkers that report error-class violations must also return a failing process status** (!23098). Added to `ci-tooling-conventions.md`.

## Strong corroboration deliberately not duplicated

- !23059 strongly reinforces the existing bounded-sub-TVB and mandatory-parser-progress guidance: malformed nested lengths should fail at the nested boundary rather than permit offset regression or infinite loops.
- !23064 independently reinforces the existing rule from later !23455 that inexpensive lower-level defensive checks can remain valuable even after the triggering higher-level bug is fixed.
- !23107, !23097, !23084 and !23101 continue the established fetch-once / proto-tree return-value-helper pattern.
- !23100, !23099, !23095, !23093, !23088, !23087, !23086, !23074 and !23071 continue the accepted unsigned-domain and remaining-aware TVBuff API migration.
- !23067, !23065 and !23092 reinforce the notebook's existing distinction between real analyzer findings, correct lifetime modeling, and false-positive suppression/modeling.

Closed !23083 was explicitly down-weighted. Backports were counted as reviewed but were not allowed to outweigh their merged master changes.
