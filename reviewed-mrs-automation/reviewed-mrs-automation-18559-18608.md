# Automated Wireshark MR review: !18559-!18608

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Review direction: descending from newest available MRs toward older MRs.

Selection method: rebuilt the already-reviewed set from the available `reviewed-mrs-automation/` ledgers and `reviewed-mrs.md`, preserving and counting the historical !17571-!17620 batch. Selected the fifty highest-numbered corpus MRs not in that set. Numeric ranges were not assumed reviewed merely from partial ledger coverage.

## Exact reviewed MR set

!18608, !18607, !18606, !18605, !18604, !18603, !18602, !18601, !18600, !18599,
!18598, !18597, !18596, !18595, !18594, !18593, !18592, !18591, !18590, !18589,
!18588, !18587, !18586, !18585, !18584, !18583, !18582, !18581, !18580, !18579,
!18578, !18577, !18576, !18575, !18574, !18573, !18572, !18571, !18570, !18569,
!18568, !18567, !18566, !18565, !18564, !18563, !18562, !18561, !18560, !18559.

Count: **50**.

## Review weighting and durable findings

Merged MRs were weighted above closed/abandoned/superseded work, and high-authority maintainer-authored/reviewed changes were given correspondingly high weight.

### !18590 — fuzzshark: use Buffer APIs

Merged, authored and merged by Guy Harris. Replaces direct manipulation of the internal `Buffer` members in `fuzzshark` with `wtap_rec_init()`, `ws_buffer_append()`, and `wtap_rec_cleanup()`. This is strong architectural evidence that callers should use the owning abstraction's public lifecycle/data APIs rather than reaching into representation fields, even when direct manipulation appears simpler or avoids a copy. It also makes initialization/cleanup symmetry explicit. This strongly corroborates existing notebook ownership/lifecycle guidance and the later Buffer/wiretap cleanup sequence already reviewed around !18614-!18622.

### !18577 — editcap: narrow an API to the data it actually needs

Merged, authored and merged by Guy Harris. `fileset_get_filename_by_pattern()` previously accepted an entire `wtap_rec *` merely to obtain a timestamp. The accepted refactor changes it to accept `const nstime_t *`, eliminating the need to manufacture a temporary `wtap_rec` when only a timestamp is available. Strong evidence for narrow interfaces: pass the semantic datum required by a helper instead of a large owning/context object when the helper does not need that object's semantics.

### !18570 — UCP short-data boundary calculation

Merged release-4.4 backport by John Thacker. Corrects a malformed-data length test so the available byte array must contain both the address-length byte and the encoded address octets. The accompanying comment explicitly records that GSM 7-bit data has no NUL terminator here. This corroborates existing defensive parsing guidance: derive bounds from actual wire representation, account separately for prefix/length bytes, and do not accidentally import C-string termination assumptions into encoded protocol data.

### !18608 — Import Text regex-mode diagnostics

Merged by John Thacker. Distinguishes characters intentionally ignored by the selected encoded-data parser (including separators/padding) from truly invalid characters that abort conversion, and warns on the latter rather than treating matched whitespace as inherently suspicious. This is useful UI/parser evidence: diagnostics should reflect parser semantics and actual failure conditions rather than lexical appearance alone.

## Notebook impact

No additional convention file was changed in this run. The strongest lessons are already represented by existing notebook guidance on abstraction ownership/lifecycle, narrow semantic interfaces, and defensive length handling. Adding duplicate prose would reduce the notebook's signal-to-noise ratio. This run ledger itself is the notebook update and records the exact reviewed set and corpus revision for duplicate avoidance.

## Continuation

On the next backward review run, rebuild the reviewed set from all tracking again. If the corpus and tracking remain otherwise unchanged, the next expected frontier is below !18559; do not blindly assume that frontier if newer unreviewed holes appear in the rebuilt set.
