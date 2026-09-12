# Automated Wireshark MR review ledger: !22141-!22190

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Review direction: newest to oldest.

Selection method: reconstructed the already-reviewed set from `reviewed-mrs-automation/` and `reviewed-mrs.md`, preserving the historical !17571-!17620 batch. Selected the fifty highest-numbered corpus MRs not already reviewed. This run contains exactly !22190 through !22141.

## Exact reviewed MR set

1. !22190
2. !22189
3. !22188
4. !22187
5. !22186
6. !22185
7. !22184
8. !22183
9. !22182
10. !22181
11. !22180
12. !22179
13. !22178
14. !22177
15. !22176
16. !22175
17. !22174
18. !22173
19. !22172
20. !22171
21. !22170
22. !22169
23. !22168
24. !22167
25. !22166
26. !22165
27. !22164
28. !22163
29. !22162
30. !22161
31. !22160
32. !22159
33. !22158
34. !22157
35. !22156
36. !22155
37. !22154
38. !22153
39. !22152
40. !22151
41. !22150
42. !22149
43. !22148
44. !22147
45. !22146
46. !22145
47. !22144
48. !22143
49. !22142
50. !22141

## Review notes and weighting

- !22190 — Deep, merged master, authored by John Thacker. HTTP/2 flow-control accounting uses checked addition, clamps impossible overflow directions, and emits expert information for protocol violations. Strong corroboration for checked arithmetic plus protocol semantic limits.
- !22189 — Scanned, merged. SGP32 CoAP URI-Path registration; protocol-specific integration, no new cross-cutting rule.
- !22188 — Deep/corroboration, merged, Michael Mann. Adds an epan-owned cached accessor so dissectors do not depend directly on application configuration APIs. Strong application-boundary evidence.
- !22187 — Scanned, merged. PFCP IE-list standards update; no new reusable lesson.
- !22186 — Scanned, merged. Windows brotli dependency update.
- !22185 — Scanned, merged. Fixes issues found by `check_typed_item_calls.py`; corroborates project checker use.
- !22184 — Scanned, merged. CMake brotli update.
- !22183 — Deep/corroboration, merged, Michael Mann. Removes preference-layer calls to application-flavor API and funnels application-specific information through epan/application interfaces.
- !22182 — Scanned, merged. PEP8 cleanup of checking scripts.
- !22181 — Scanned, merged. ANSI MAP/TCAP bitmask fixes.
- !22180 — Discussion/diff scan, merged. Adds CoAP URI-Path dissector dispatch and Message-ID fallback transaction identity when token is absent; useful protocol-specific dispatch/state evidence but no separate rule promoted.
- !22179 — Deep/corroboration, merged, John Thacker. OSS-Fuzz signed-overflow fix around TVBuff remaining length; reinforces domain-correct integer handling.
- !22178 — Deep/corroboration, merged, John Thacker. Replaces unnecessary explicit captured-length subset construction with `tvb_new_subset_length`; reinforces captured-vs-reported TVBuff semantics.
- !22177 — Deep/corroboration, merged, Michael Mann. Begins concentrating application-flavor handling at epan boundary rather than throughout epan internals.
- !22176 — Scanned, merged. ASAM CMP filter corrections.
- !22175 — Deep/corroboration, merged, Michael Mann. Removes Wiretap calls to application-flavor API; preserves lower-layer dependency direction.
- !22174 — Scanned, merged. Silabs DCH IEEE 802.15.4 FCS support.
- !22173 — Deep/corroboration, merged. More conversions away from `tvb_new_subset_length_caplen(..., -1)` patterns; same TVBuff semantics as !22171/!22178.
- !22172 — Deep/corroboration, merged. ISUP moves to registered typed item extraction so values work consistently in filters/custom columns; reinforces typed-field semantics.
- !22171 — Deep, merged. Explains why `tvb_new_subset_length()` should preserve the requested semantic reported length on truncated captures and defer BoundsError until unavailable bytes are actually accessed, allowing maximal partial dissection. Strong TVBuff/truncation exemplar.
- !22170 — Scanned, merged. Misc GLib-to-C99 integer type conversion.
- !22169 — Scanned, merged. CMake brotli update.
- !22168 — Scanned, merged. Factors common functions among checking tools.
- !22167 — Scanned, merged. EAPOL MKA SAK unwrapping-length fix.
- !22166 — Deep, merged. BER OSS-Fuzz signed-overflow fix uses TVBuff reported-length validation before offset propagation; reinforces validating parser coordinates before arithmetic escapes their semantic domain.
- !22165 — Deep/corroboration, merged, Michael Mann. Removes wsutil calls to application-flavor API and explicitly states the goal of linking application-flavor policy only at application level.
- !22164 — Scanned, merged. DHCPv6 Clang Analyzer dead-store cleanup.
- !22163 — Deep, merged, John Thacker co-authored/merged. Replaces MKA use/destruction of shared `pinfo->private_table` with protocol-scoped proto data allocated in `pinfo->pool`; promoted to `dissector-state-conventions.md` for ownership and exception safety.
- !22162 — Scanned, merged. SGP22 ASN.1 generation fix.
- !22161 — Scanned, merged. Version-info memory-leak fix.
- !22160 — Scanned/corroboration, merged, Martin Mathieson. Improves project checking tools' coverage of plugin dissectors; reinforces that plugin code should receive the same static/checker scrutiny as built-in dissectors.
- !22159 — Scanned, merged. IPv4 reserved-flag standards wording clarification.
- !22158 — Deep, merged, John Thacker; approved/merged by Jaap Keuter. Restores VLAN protocol depth on exit so depth represents nesting rather than cumulative invocations and removes TECMP's VLAN-specific workaround. Promoted to `dissector-state-conventions.md`.
- !22157 — Deep, merged, John Thacker; approved/merged by Jaap Keuter. Replaces a packed nonce struct with the exact byte-array representation required by the crypto API, avoiding unnecessary host-endian/alignment semantics. Corroborates existing serialized-layout-vs-C-structure guidance.
- !22156 — Scanned, closed/unmerged. Proposed checker filtering change; down-weighted because it was not accepted.
- !22155 — Scanned, merged. ASAM CMP GigE Vision/MIPI CSI-2 support.
- !22154 — Scanned, merged. CMake ordering correction.
- !22153 — Scanned, merged. Kerberos4 dissector return-type cleanup.
- !22152 — Scanned, merged. CTDB dissector return-type cleanup.
- !22151 — Scanned, merged. Documentation/layout cleanup.
- !22150 — Scanned, merged. Plugin GLib-to-C99 integer conversion.
- !22149 — Scanned, merged. wsutil GLib-to-C99 integer conversion.
- !22148 — Scanned, merged. Wiretap GLib-to-C99 integer conversion.
- !22147 — Scanned, merged. epan GLib-to-C99 integer conversion.
- !22146 — Scanned, merged. ASAM CMP code-quality cleanup.
- !22145 — Scanned, merged. Narrows ORAN helper linkage with `static`.
- !22144 — Scanned, merged. Automatic data/translation update.
- !22143 — Scanned, merged. Automatic data/translation update.
- !22142 — Scanned, merged. Automatic data/translation update.
- !22141 — Deep, merged. MIDI SysEx shared database/generator. Michael Mann explicitly says protocol-specific shared code/data should stay under `epan/dissectors/`, with `epan/` reserved for the generic dissection engine; promoted to `application-layer-boundary-conventions.md`. Review also simplified an unnecessary generated-file split.

## Notebook changes

- `dissector-state-conventions.md`: added protocol-scoped transient state guidance from !22163 and stack-like restoration of temporary protocol-depth state from !22158.
- `application-layer-boundary-conventions.md`: added Michael Mann's !22141 placement rule that shared protocol-specific code/data belongs with dissectors rather than generic epan, and recorded !22165/!22175/!22177/!22183/!22188 as corroborating application-boundary lineage.
- Existing TVBuff, arithmetic, field-semantic, serialized-layout, testing, and checker conventions were corroborated where applicable rather than duplicated.

Historical !17571-!17620 remains preserved and counted in the already-reviewed set.