# Automated MR review ledger: !13063–!13112

Reviewed 2026-09-22 using GPT-5.6 Sol.

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Exactly 50 previously unreviewed merge requests were reviewed in this run. The already-reviewed set was rebuilt from the available `reviewed-mrs-automation/` tracking and `reviewed-mrs.md`; the historical !17571–!17620 batch was explicitly preserved and counted. The immediately preceding ledger had only inspected !13112 as a frontier probe, so it was not treated as reviewed. The selected numbers happen to be contiguous, but selection was made from the explicit reviewed set and corpus membership rather than by assuming range coverage.

Outcome summary: 47 merged, 2 closed/unmerged (!13090 and !13086), and 1 still-open corpus snapshot (!13087). Merged changes were weighted most heavily; the closed/open TLS-glob proposals were treated as weak/superseded evidence. Guy Harris's feedback on !13090 and his authored/merged !13074/!13075 linker workaround were given appropriate maintainer weight.

| MR | Outcome | Depth | Review result |
|---|---|---|---|
| !13112 | merged | Scanned | `epan: Convert uat.h indentation to 4-space`; mechanical formatting cleanup, no durable rule beyond existing style guidance. |
| !13111 | merged | Deep | John Thacker's TShark delimiter-separated escaping fix composes repeated values before escaping/quoting the complete logical field and centralizes grammar in `ws_escape_csv()`. Promoted to `generated-text-output-conventions.md`. |
| !13110 | merged | Scanned | `UAT: Add uat_destroy()`; explicit destruction API/lifecycle cleanup, useful corroboration of ownership conventions but no new rule promoted. |
| !13109 | merged | Discussion-focused | Removes the deprecated combined `filters` fallback and documents/comments cfilters/dfilters. Bot requested trivial commits be squashed; behavior cleanup is narrow. |
| !13108 | merged | Discussion-focused | PFCP Outer Header Creation correction after the author identified an earlier wrong interpretation. Pascal Quantin requested keeping value tables ordered and removing unexplained trailing spaces. Good table/review hygiene; existing conventions suffice. |
| !13107 | merged | Scanned | GTPv2 cleanup comments an unused `hf` entry; maintenance only. |
| !13106 | merged | Scanned | GTPv2 field-description trailing-colon cleanup; presentation-only. |
| !13105 | merged | Discussion-focused | GTPv2 SCEF Reference ID Extended support; most discussion concerned getting contributor CI to run successfully, then the change passed. No new cross-cutting convention. |
| !13104 | merged | Deep/corroboration | Tooling portion of the static protocol/field initialization migration; ensures generated/tool output follows the new zero-initialized registration invariant. Promoted with !13091–!13093. |
| !13103 | merged | Scanned | Tests switch home-directory setup to pytest `tmp_path`; preferred framework-managed temporary-path hygiene, but no separate notebook rule needed. |
| !13102 | merged | Scanned | UAT removes unused `from_global` boolean state; straightforward dead-state cleanup. |
| !13101 | merged | Scanned | GitLab CI external-project commit check tolerates unavailable `GITLAB_TOKEN`/`glab` conditions instead of making an optional integration brittle. Existing CI robustness guidance covers it. |
| !13100 | merged | Scanned | GitLab CI path correction; infrastructure maintenance only. |
| !13099 | merged | Deep | Radius AVP decryption expands RFC 2868/Ascend and non-string support; encrypted fields were made explicit for affected types. Solid protocol implementation, no new cross-cutting rule extracted. |
| !13098 | merged | Scanned | Release-4.0 Windows GnuTLS package refresh/custom packaging; dependency maintenance. |
| !13097 | merged | Scanned | CI runs `tshark --version` after macOS and Windows packaging, providing a cheap packaged-binary smoke test. Corroborates existing post-build validation practice. |
| !13096 | merged | Scanned | Release-3.6 backport of ISO15765 Info-column duplicate-text fix; duplicate evidence of !13082. |
| !13095 | merged | Scanned | Release-4.0 backport of ISO15765 Info-column duplicate-text fix; duplicate evidence of !13082. |
| !13094 | merged | Scanned | Release-4.2 backport of ISO15765 Info-column duplicate-text fix; duplicate evidence of !13082. |
| !13093 | merged | Deep/corroboration | Plugin portion of static protocol-value zero-initialization migration. Promoted with !13091/!13092/!13104. |
| !13092 | merged | Deep | Preparatory change audits validity checks so zero is invalid before declarations migrate away from explicit `-1`. Establishes the safe ordering for a sentinel/invariant migration. |
| !13091 | merged | Deep | Core epan migration to zero-initialized static protocol/hf/expert state; cached protocol handle changes from `== -1` to semantic `<= 0` invalid / `> 0` valid. Promoted to `registration-handle-initialization-conventions.md`. |
| !13090 | closed, unmerged | Deep/discussion | Proposed defaulting file-pcap/pcapng/btsnoop to “Dissect next layer.” Pascal Quantin challenged the mode semantics; Guy Harris emphasized that either default serves different workflows and questioned using file-format dissection for ordinary packet decoding. Anders closed it because the alternate behavior already existed as a preference. Promoted as review-policy evidence, explicitly down-weighted as unmerged. |
| !13089 | merged | Deep | John Thacker reworks Ethernet padding/trailer/FCS handling and converts PRP to a trailer heuristic. Carries payload origin/length explicitly, avoids FCS computation on incomplete frames, and tries the non-FCS trailer interpretation before destructively stripping an uncertain FCS. Promoted to `framing-boundary-conventions.md`. |
| !13088 | merged | Deep | John Thacker fixes MACsec short-frame FCS handling using `set_actual_length()` and marks the ICV as appendix. Strong corroboration of semantic frame-boundary guidance. |
| !13087 | opened snapshot | Low | WIP TLS glob-pattern support for multiple keylog files. Never merged in this corpus snapshot; down-weighted and not used as durable implementation evidence. |
| !13086 | closed, unmerged | Low | Earlier WIP TLS keylog-glob proposal, quickly closed/superseded by !13087. No durable rule extracted. |
| !13085 | merged | Deep | Adds InfiniBand FLUSH and ATOMIC Write from IBTA specifications with concrete tshark examples and a sample capture-oriented validation narrative. Strong protocol-submission example; existing testing/spec-source guidance covers it. |
| !13084 | merged | Scanned | ISO15765 STmin correction: 0xF1–0xF9 represent 100–900 µs in 100-µs steps. Specification correctness fix. |
| !13083 | merged | Scanned | BMP v4 capability expert-info bug caused by reading a one-byte value with a two-byte accessor. Useful width/accessor correctness example; existing typed/field-width guidance covers it. |
| !13082 | merged | Scanned | Master ISO15765 duplicate Info-column text fix; localized presentation correction, followed by three stable backports. |
| !13081 | merged | Scanned | Release-4.2 MIKEY fix skips unknown payload types consistently with out-of-range payload handling. Corroborates graceful unknown-enum handling. |
| !13080 | merged | Deep/corroboration | Release-4.2 backport of Thrift field-ID state fix; explicitly notes fuzz testing passed. Corroborates !13063 and parser-state transition guidance. |
| !13079 | merged | Scanned | Display-filter registration validates function names. Existing identifier-validation guidance covers it. |
| !13078 | merged | Scanned | Release-4.2 backport of display-filter scanner fix for protocol names beginning with a digit. Duplicate of !13073. |
| !13077 | merged | Discussion-focused | GTPv2 SCEF Reference ID support. Review preserved a useful octet-layout comment and corrected field type to `FT_UINT64`; reinforces spec comments and exact field typing without adding a new rule. |
| !13076 | merged | Scanned | GitLab CI conditionally installs `glab`; dependency/bootstrap robustness only. |
| !13075 | merged | Scanned/high-authority | Guy Harris-authored release-4.2 backport working around Xcode 15's linker when building GMP by selecting the old linker. Narrow platform compatibility fix; no broad convention promoted. |
| !13074 | merged | Scanned/high-authority | Guy Harris-authored master Xcode 15 linker workaround corresponding to !13075. Strong authoritative evidence, but intentionally kept as platform-specific implementation knowledge rather than generalized. |
| !13073 | merged | Scanned | Master display-filter scanner fix permits valid protocol names that begin with a digit. Lexer/identifier edge case already covered by existing display-filter guidance. |
| !13072 | merged | Deep | John Thacker sets both the HomePNA proto-item length and tvbuff actual length when the protocol ends early, keeping UI byte ownership and outer Ethernet padding/trailer/FCS logic synchronized. Promoted to `framing-boundary-conventions.md`. |
| !13071 | merged | Scanned | Automated master registry/manuf/translation update; generated-data maintenance only. |
| !13070 | merged | Scanned | Automated release-4.2 registry/manuf/translation update; generated-data maintenance only. |
| !13069 | merged | Scanned | Automated release-3.6 registry/manuf/translation update; generated-data maintenance only. |
| !13068 | merged | Scanned | Automated release-4.0 registry/manuf/translation update; generated-data maintenance only. |
| !13067 | merged | Discussion-focused | Adds SMB2 POSIX filesystem info with a demo capture. Alexis La Goutte notes that old `parent_tree` guards no longer provide the historical speed benefit; useful cleanup guidance, but the MR's primary change is protocol-specific. |
| !13066 | merged | Scanned | Release-3.6 backport adding OpenFlow v1 FLOW_MOD actions; duplicate of the established master implementation. |
| !13065 | merged | Scanned | Release-4.0 backport adding OpenFlow v1 FLOW_MOD actions; duplicate implementation evidence. |
| !13064 | merged | Scanned | Release-4.2 backport adding OpenFlow v1 FLOW_MOD actions; duplicate implementation evidence. |
| !13063 | merged | Deep | Thrift helper family now updates `previous_field_id` itself after successful field parsing, preventing missing caller bookkeeping and nested-state leakage from producing false unordered-field diagnostics. Promoted to `parser-state-transition-conventions.md`. |

## Durable notebook updates

- `generated-text-output-conventions.md`: delimiter-separated output must compose one logical field before applying format-level quoting/escaping; shared escaping API, docs, symbols, and tests should agree (!13111).
- `parser-state-transition-conventions.md`: helper APIs that consume a semantic field should own the corresponding parser-state transition; nested child state must not leak into the parent level (!13063, !13080).
- `framing-boundary-conventions.md`: padding/trailer/FCS classification must use semantic payload/frame boundaries and captured/reported length state; uncertain FCS handling should avoid destructive guesses; tvbuff actual length and proto-item span must stay synchronized (!13089, !13088, !13072).
- `registration-handle-initialization-conventions.md`: zero-initialized static registration state is safe only after validity predicates and generators/plugins are audited as part of the same invariant migration (!13092, !13091, !13093, !13104).
- `default-behavior-conventions.md`: a default should match the semantics of the selected mode rather than a specialized opt-in workflow; !13090 is explicitly recorded as review-policy evidence, not merged implementation precedent, with Guy Harris/Pascal Quantin/Anders Broman feedback weighted accordingly.

## Frontier

`!13062` (`WebSocket: Make the max unmasked payload length configurable`) exists in the same corpus commit and is merged. It was inspected only to verify that the corpus continues below this batch and is **not** counted as reviewed here. Absent newly scraped higher-numbered unreviewed material, it is the next descending candidate.