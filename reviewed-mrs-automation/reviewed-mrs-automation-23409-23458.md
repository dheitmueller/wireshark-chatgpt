# Automated Wireshark MR review: !23409-!23458

- Corpus: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`
- Review direction: newest to older
- Exact MRs reviewed in this run: 50
- Selection method: rebuilt the already-reviewed set from the available `reviewed-mrs-automation/` ledgers plus explicit entries in `reviewed-mrs.md`; selected the 50 highest-numbered corpus MRs not in that set. Numeric ranges were not treated as reviewed unless their individual entries were represented by tracking.
- Historical !17571-!17620 batch: preserved and counted as previously reviewed.

## Exact reviewed set

| MR | Review depth | Outcome / review note |
|---|---|---|
| !23458 | Scanned | Merged John Thacker EPMD fix corrects `tvb_find_line_end_remaining` argument order and uses captured-length termination; reinforces existing captured-vs-reported/search guidance. |
| !23457 | Scanned | Merged broad conversion to unsigned `tvb_find_uint8`/offset APIs; reinforces existing unsigned-offset and remaining-length guidance. |
| !23456 | Scanned | Merged NAS EPS specification update to v19.5.0; protocol-content update with no additional durable review convention. |
| !23455 | Deep | Merged BLF writer crash fix. Guy Harris explicitly said the mapping-result check should remain even after !23488 fixes the underlying invariant, because it converts a future violation into an internal error rather than a crash. Promoted to `wiretap-writer-conventions.md`. |
| !23454 | Scanned | Merged `validate-commit` AI-Assisted trailer/help ordering cleanup; corroborates existing submission/commit-message validation practice. |
| !23453 | Scanned | Merged `validate-commit` comment/newline normalization; tooling implementation detail, no distinct durable rule. |
| !23452 | Deep | Merged public pcapng header fix replaces C++ keyword `new`; Guy Harris also reviewed replacement naming. Promoted public C/C++ header-compatibility rule to `api-design-conventions.md`. |
| !23451 | Scanned | Merged release-4.6 backport raising minimum macOS version for Qt 6.10; no additional lesson beyond accepted master/build compatibility behavior. |
| !23450 | Scanned | Merged Zigbee Direct context/state fix plus protocol-column fencing; reinforces existing downstream-context and nested-dissector presentation guidance. |
| !23449 | Scanned | Merged unsigned `tvb_find_uint8`/offset migration; corroborates existing signed/unsigned offset guidance. |
| !23448 | Scanned | Merged Art-Net boolean true/false-string correction; narrow protocol-display fix. |
| !23447 | Scanned | Merged `.mailmap` maintenance with typo review; no reusable engineering convention. |
| !23446 | Scanned | Merged `tvb_find_line_end_remaining` and unsigned-offset migration across dissectors; corroborates existing remaining-length/API modernization guidance. |
| !23445 | Scanned | Merged removal of repeated dissector fetches; reinforces the already-recorded fetch-once/ret-value helper convention. |
| !23444 | Scanned | Merged additional `tvb_find_uint8` unsigned/remaining migration; no new rule. |
| !23443 | Scanned | Merged RSA simplification using `gnutls_pkcs12_simple_parse`; library/API cleanup without distinct review convention. |
| !23442 | Scanned | Merged stable-branch counterpart of the GnuTLS allocator/free correction; counted as corroboration, not a separate rule. |
| !23441 | Scanned | Merged stable-branch counterpart of the GnuTLS allocator/free correction; counted as corroboration, not a separate rule. |
| !23440 | Deep | Merged John Thacker fix uses `gnutls_free()` for buffers returned by GnuTLS; strong corroboration of the notebook's existing allocator-family/ownership rule, especially across Windows CRT boundaries. |
| !23439 | Scanned | Merged TLS/RSA implementation compatibility change reverting pre-master-secret decryption to libgcrypt; crypto-backend-specific, no general rule promoted. |
| !23438 | Scanned | Merged QCDIAG command additions; protocol implementation with no durable human-review lesson. |
| !23437 | Scanned | Merged automatic data/translation update; no engineering convention. |
| !23436 | Scanned | Merged automatic data/translation update; no engineering convention. |
| !23435 | Scanned | Merged automatic data/translation update; no engineering convention. |
| !23434 | Scanned | Merged `rsa_decrypt_inplace` modernization using the crypto library's padding support; implementation cleanup, no new convention. |
| !23433 | Scanned | Merged stable-branch CI virtual-environment cleanup; no additional lesson beyond master counterpart. |
| !23432 | Scanned | Merged Qt palette-change style-sheet refresh fix; narrow UI lifecycle correction. |
| !23431 | Scanned | Merged NATS `proto_tree_add_item` length fix correcting offset-vs-length confusion; straightforward API correctness, no new general rule. |
| !23430 | Scanned | Merged `tvb_find_uint8_remaining()` conversion; reinforces existing TVB search guidance. |
| !23429 | Scanned | Merged Qt style-sheet propagation application attribute; UI-specific behavior, no general convention promoted. |
| !23428 | Deep | Merged John Thacker change adds `DISSECTOR_ASSERT(tvb)` to low-level `tvb_get_ptr()`, where NULL is necessarily a programmer error. Paired with !23427 and promoted to assertion/API-layer guidance. |
| !23427 | Deep | Merged John Thacker fix preserves `proto_tree_add_bytes*`'s established NULL-tvb behavior while guarding the call to stricter `tvb_get_ptr()`. Paired with !23428 and promoted to assertion/API-layer guidance. |
| !23426 | Scanned | Merged supported-branch GLib bundle update/fixup; dependency maintenance only. |
| !23425 | Scanned | Merged CI cleanup clears/recreates Python virtual environments; useful hygiene but already covered by CI-environment reproducibility guidance. |
| !23424 | Scanned | Merged stable-branch WinSparkle package/layout update; dependency packaging maintenance. |
| !23423 | Deep | Merged John Thacker Qt fix makes RSA-key-list changes trigger redissection. Strong corroboration of existing preference-state guidance that all dissection-changing effects, including the RSA Keys panel, must contribute redissection effects. |
| !23422 | Scanned | Merged branch counterpart of GnuTLS signed-status return fix; corroborates existing rule not to collapse status/error domains into boolean truthiness. |
| !23421 | Scanned | Merged branch counterpart of GnuTLS signed-status return fix; same already-recorded status-return lesson. |
| !23420 | Scanned | Merged IKEv2 emergency-call-number decoding fix and common-code reuse; protocol-specific, no new rule. |
| !23419 | Deep | Merged master GnuTLS status-return fix preserves negative error values instead of casting them to bool. Strong corroboration of existing explicit status-domain/boolean guidance; not duplicated. |
| !23418 | Scanned | Merged CMake indentation cleanup; style-only. |
| !23417 | Scanned | Merged QCDIAG Coverity DEADCODE fixes; corroborates existing static-analysis guidance without adding a distinct rule. |
| !23416 | Scanned | Merged stable-branch c-ares dependency update; maintenance only. |
| !23415 | Scanned | Merged CI adjustment for Qt 6.10 requiring macOS deployment target 13; build compatibility maintenance. |
| !23414 | Scanned | Merged additional dissector double-fetch removals; reinforces existing fetch-once guidance. |
| !23413 | Scanned | Merged FetchArtifacts CMake copy/paste package-name typo fix; narrow tooling correction. |
| !23412 | Scanned | Merged branch counterpart of IKEv2 emergency-call-number decoding fix; no additional lesson. |
| !23411 | Scanned | Merged GLib bundle update for macOS/Windows; dependency maintenance. |
| !23410 | Scanned | Merged FetchArtifacts comment update; documentation-only. |
| !23409 | Scanned | Merged Gerald Combs documentation fix for the Transifex URL; no engineering convention. |

## Promoted durable conventions

1. **Defensive containment remains valuable after root-cause repair.** From merged !23455 and direct Guy Harris guidance: retain checks on fallible writer/helper operations when they convert an unexpected invariant violation into a controlled internal error instead of a crash, even if another fix should normally make the condition impossible.
2. **Public C headers must honor the C++ compatibility they advertise.** From merged !23452: a C header intended for C++ inclusion must avoid C++-reserved identifiers throughout its public surface.
3. **Put assertions at the layer where a violation is truly a programmer error without narrowing broader wrapper contracts.** From merged !23427/!23428: preserve higher-level APIs that intentionally accept NULL/sentinel inputs, guard calls into stricter primitives, and assert the primitive's non-NULL invariant there.

## Strong corroboration intentionally not duplicated

- !23440 reinforces allocator-family matching (`gnutls_malloc`/`gnutls_free`) and highlights Windows CRT-boundary consequences.
- !23423 reinforces the already-recorded preference-effect/redissection ordering rule, including RSA key changes.
- !23419/!23421/!23422 reinforce preserving the full signed status/error domain rather than coercing it to bool.
- !23445/!23414 and the TVB migration series reinforce fetch-once, remaining-length, captured-length, and unsigned-offset conventions already present in the notebook.
