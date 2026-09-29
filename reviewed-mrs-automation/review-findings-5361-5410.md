# Wireshark MR review findings 5361-5410

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

| MR | Outcome | Review depth | Finding |
|---|---|---|---|
| !5410 | merged | Focused | Extends JPEG/Exif dissection to nested Exif/GPS/Interoperability IFDs. Recursive IFD pointers are followed only after checking the target offset is within the reported tvbuff length; separate hf fields preserve the semantic tag namespace for each IFD type. |
| !5409 | merged | Focused | Reduces CMake/CI command verbosity while preserving warnings/errors. Useful CI signal-to-noise cleanup, but no distinct architecture rule beyond keeping diagnostics visible. |
| !5408 | merged | Stable backport | release-3.4 cherry-pick of !5405's GitLab CI credential-check simplification. Lower weight than master. |
| !5407 | merged | Stable backport | release-3.6 cherry-pick of !5405. Lower weight than master. |
| !5406 | merged | Focused | Normalizes extcap-base indentation and removes its obsolete EditorConfig exception. Style/tooling cleanup only. |
| !5405 | merged | Focused | Simplifies GitLab CI upload checks by treating the destination variable as the contract that credentials were configured. CI environment-specific; no broader rule promoted. |
| !5404 | merged | Focused | PCEP RP parsing advances past the fixed RP body, subtracts header/fixed lengths from the object length, and sends only the remaining bytes to the common TLV dissector. Reinforces bounded nested parsing. |
| !5403 | merged | Stable backport | release-3.6 backport of !5402 IEC101/104 configurable link-address-size support. |
| !5402 | merged | Deep corroboration | John Thacker makes IEC101 framing honor the configured 0/1/2-byte link-address width in both parsing and ASDU-length calculation, and removes an unnecessary apply-prefs shim. Corroborates the later framing-length rule from !5521: active protocol preferences that change framing must feed the actual length calculation. |
| !5401 | merged | Focused | RTSP CLI statistics now print actual response packet counts and align request/response columns. User-visible tap-output correctness only. |
| !5400 | merged | Stable documentation | release-3.4 documentation backport describing TShark statistics and their independence from the display filter. |
| !5399 | merged | Focused | Restores an accidentally omitted SMB statistics option in the TShark manual. Documentation completeness only. |
| !5398 | merged | Focused | Removes dead stores found by Clang Analyzer in MBIM/MKA. Straight static-analysis cleanup; no new rule beyond treating analyzer warnings as concrete review inputs. |
| !5397 | merged | Deep / historical architecture | Removes the experimental parallel proto-tree registration API and its exported symbol, returning netlink users and tooling to the canonical hf-index registration model. Strong historical evidence that an experimental alternate API should not remain as a second long-term path once the project decides against it. |
| !5396 | merged | Stable documentation | release-3.6 backport of !5392 TShark statistics documentation. |
| !5395 | merged | Historical migration | Converts netlink dissectors and helpers from direct header_field_info plumbing to ordinary hf-index registration and removes proto_register_fields_manual(). Part of the same canonicalization sequence completed by !5397. |
| !5394 | merged | Deep discussion | Npcap 1.60 packaging update. Jörg Mayer questioned what “previously shipped” should mean; Pascal Quantin confirmed the release note should compare against the actual 3.6 shipped Npcap 1.55 baseline. User-facing release notes should describe the previous released product, not merely the immediately preceding development commit. |
| !5393 | merged | Deep | João Valverde implements compatibility for legacy `-o console.log.level` inside wslog argument parsing, maps the old bitmask to the new log level, emits a deprecation message, and leaves the general obsolete-preference policy unchanged. The accepted shim is narrower than closed !5380. |
| !5392 | merged | Focused | Master documentation update enumerating TShark statistics and clarifying how capture/read/stat filters interact with display filtering. |
| !5391 | merged | Stable backport | release-3.4 backport of !5383 RTSP token-boundary fix. |
| !5390 | merged | Stable backport | release-3.6 backport of !5383 RTSP token-boundary fix. |
| !5389 | merged | Stable backport | release-3.4 backport of !5384 NULL-filter RTSP CLI tap fix. |
| !5388 | merged | Stable backport | release-3.6 backport of !5384. |
| !5387 | merged | Historical migration | Converts another group of dissectors from the experimental proto-tree API back to ordinary hf registration. Corroborates !5397. |
| !5386 | merged | Historical migration | Additional dissector conversions away from the experimental proto-tree API. Corroborates !5397. |
| !5385 | merged | Focused | Jaap Keuter simplifies README.dissector by keeping representative proto-tree examples and referring readers to proto.h for the complete API instead of duplicating a massive function inventory. Moshe Kaplan suggests broader Doxygen/WSDG consolidation. |
| !5384 | merged | Deep | John Thacker fixes the RTSP CLI tap's no-filter crash by treating the filter pointer itself as optional before testing its first character. Optional configuration values must distinguish NULL from a present empty string unless the API explicitly guarantees otherwise. |
| !5383 | merged | Deep | John Thacker fixes RTSP status parsing by passing `line + linelen` as the end pointer to get_token_len() instead of an arbitrary `line + 5`. A tokenizer taking a bounded range must receive the real containing range; a guessed end can silently truncate a token and misclassify the packet. |
| !5382 | merged | Historical migration | Converts another dissector group back to ordinary proto-tree registration. Corroborates !5397. |
| !5381 | merged | Historical migration | Converts Redback/Rsync/Rwall/STAT/TALI/XCSL away from the experimental proto-tree API. Corroborates !5397. |
| !5380 | closed/unmerged | Superseded discussion | Gerald Combs proposed a broader legacy console.log.level compatibility patch. João Valverde clarified historical stdout/stderr behavior and recommended implementing the compatibility translation in ws_log_parse_args(); merged !5393 is the accepted successor and authoritative design. |
| !5379 | merged | Deep corroboration | Fixes Qt compilation when libpcap is disabled by guarding capture-options-dialog use with HAVE_LIBPCAP. Strong corroboration for the minimal-feature CI rule: optional-feature-off builds expose hidden compile-time dependencies that normal builds miss. |
| !5378 | merged | Scanned | Removes the conversion script after the experimental proto-tree API migration was reversed. Cleanup after !5397. |
| !5377 | merged | Scanned | Corrects executable/file modes on ASN.1/dissector sources. Repository hygiene only. |
| !5376 | merged | Historical migration | SLL conversion away from the experimental proto-tree API, including a pre-commit-check fix. |
| !5375 | merged | Historical migration | RIP conversion away from the experimental proto-tree API, including a pre-commit-check fix. |
| !5374 | merged | Historical migration | VLAN conversion away from the experimental proto-tree API. |
| !5373 | merged | Deep discussion | UDP conversion plus 2-space-to-4-space reformat. Jaap Keuter's review repeatedly prefers natural continuation alignment/unwrapping over mechanical tab-stop alignment; João applies the requested fixes while distinguishing block indentation from continuation indentation. Style evidence, not a new architecture rule. |
| !5372 | merged | Deep | Gerald Combs changes the Qt feature check from `VERSION_GREATER 5.10` to `NOT VERSION_LESS "5.11"`. If a feature starts at 5.11, testing “greater than 5.10” incorrectly admits patch releases such as 5.10.1; encode the true minimum version boundary directly. |
| !5371 | merged | Focused | Initializes rawshark's `fs_ptr` to NULL to eliminate a possible-uninitialized path found by the compiler. Local defensive initialization. |
| !5370 | merged | Stable backport | release-3.6 backport of the obsolete-preference message improvement together with the tfshark error-path correction. |
| !5369 | merged | Deep follow-up | Restores tfshark's PREFS_SET_SYNTAX_ERR diagnostic/cleanup path accidentally removed by !5367. Cross-frontend option handling should be reviewed as a semantic matrix, not by copy/paste similarity alone. |
| !5368 | merged | Stable backport | release-3.6 QUIC version-display correction adding draft-33/34 labels. Protocol data update only. |
| !5367 | merged | Deep / negative follow-up evidence | Improves CLI wording by distinguishing unknown from obsolete preferences across rawshark/tfshark/tshark, but the tfshark edit accidentally deletes the syntax-error case. !5369 immediately repairs it. Multi-frontend mechanical edits need negative-path parity checks. |
| !5366 | closed/unmerged | Superseded | Alternative IEEE 802.11 action-frame regression fix. John Thacker points out merged !5363 already solves the issue; contributor confirms and closes this MR. Not implementation precedent. |
| !5365 | merged | Stable backport | release-3.6 backport of StatsTree collapse/expand context-menu feature. UI feature backport only. |
| !5364 | merged | Stable backport | release-3.6 backport of !5363 association-sanity context propagation. |
| !5363 | merged | Deep | John Thacker fixes an 802.11 regression introduced when management-action handling was extracted: the new helper failed to receive and forward `association_sanity_check_t`. Refactors must propagate ancillary parse/context state through every new wrapper, using NULL only for entry points where the context truly does not exist. |
| !5362 | merged | Focused | Removes display-filter tests that were permanently skipped because the tested functionality did not exist. Unconditional skipped tests for nonexistent behavior are not meaningful coverage and should not accumulate indefinitely. |
| !5361 | closed/unmerged | Discussion-only | Experimental display-filter field-reference redesign replacing macro-tree caching. Author notes documentation is still TODO and performance benefit is not measurable. Closed/unmerged, so no proposed architecture is promoted. |
