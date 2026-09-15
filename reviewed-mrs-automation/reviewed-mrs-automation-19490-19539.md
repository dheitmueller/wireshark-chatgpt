# Wireshark MR automation review: !19490-!19539

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Reviewed exactly 50 previously unreviewed merge requests, selected as the highest-numbered corpus MRs not present in the accumulated review tracking. The historical !17571-!17620 batch remains counted as reviewed. Merged MRs were weighted more heavily than abandoned/superseded work; substantive maintainer discussion and final diffs received deeper attention than routine backports, dependency updates, automatic data updates, and mechanical cleanup.

Exact reviewed set (descending):

`!19539, !19538, !19537, !19536, !19535, !19534, !19533, !19532, !19531, !19530, !19529, !19528, !19527, !19526, !19525, !19524, !19523, !19522, !19521, !19520, !19519, !19518, !19517, !19516, !19515, !19514, !19513, !19512, !19511, !19510, !19509, !19508, !19507, !19506, !19505, !19504, !19503, !19502, !19501, !19500, !19499, !19498, !19497, !19496, !19495, !19494, !19493, !19492, !19491, !19490`

## Durable findings

- **!19496 — Deep, merged, John Thacker-authored.** UDP ports 7 and 9 are registered for Echo/Discard but are commonly used for Wake-on-LAN. Echo/Discard never reject a packet, while WOL has strong recognition and rejects nonmatches. The accepted design tries WOL first and falls back to the generic service dissector. Added to `dissector-conventions.md`: when protocols share a port, a discriminating/rejecting dissector must run before a catch-all that would otherwise make later recognition unreachable.
- **!19536 — Deep, merged, Jaap Keuter-authored.** Removed fragile dissector-init-time attempts to migrate ancient scalar preferences into UATs. The MR explicitly cites the lack of well-defined migration infrastructure and inconsistent user behavior. Useful lifecycle/preference-maintenance evidence: do not invent ad-hoc migration paths in dissector initialization; obsolete ancient preferences when reliable migration cannot be guaranteed.
- **!19514 — Deep, merged.** TLS decrypted-handshake regression: encryption-state heuristics must not be re-applied after Wireshark has already established that the presented bytes are decrypted. The fix gates the encrypted-message heuristic on the caller's `maybe_encrypted` state and removes a now-redundant parameter. Good state/representation-boundary exemplar; no new notebook rule needed.
- **!19523 — Discussion-focused/deep, merged.** RDP multi-message/info-column and channel improvements. Alexis La Goutte caught a concrete `proto_tree_add_int` field-length mismatch during review (`FT_INT16` field with length 12), reinforcing that tree-add API length/type consistency is reviewable semantic correctness, not cosmetic metadata.
- **!19497 — Deep, merged, John Thacker-authored.** JSON output must remain structurally valid when an ftype has no string representation. The writer now emits JSON `null` rather than silently writing nothing; FT_NONE no longer needs a special case. Strong output-contract evidence, but no additional Wireshark-specific convention promoted.
- **!19538 — Deep, merged, John Thacker-authored.** Added explicit `nstime_t` rounding by timestamp precision while documenting that `nstime_t` itself does not carry source precision. Useful API-semantic exemplar: do not infer precision metadata from a value representation that does not encode it.
- **!19530 — Scanned, merged.** Re-enables GCC format-truncation warnings after the tree was made clean enough to build with them under `-Werror`; corroborates warning-policy/toolchain hygiene.
- **!19526 / !19528 / !19529 / !19498 — Scanned, merged correctness hardening.** Null-check and static-analysis fixes; consistent with existing defensive-programming guidance.
- **!19525 / !19501 / !19495 — Scanned, merged.** Avoid fixed-buffer formatting patterns that trigger truncation warnings; use allocation/string helpers where the output length is naturally dynamic.
- **!19512 — Scanned, merged.** USB PTP regression after URB/conversation-data separation; useful reminder to validate state ownership after refactors, already covered by lifecycle guidance.
- **!19511 / !19510 — Scanned.** PTPv2 Authentication TLV work/successive submission; protocol-specific implementation evidence, no new durable rule promoted.
- **!19490 / !19491 — Scanned backports.** Lua bit-buffer overflow fix; backport pair adds no new lesson beyond the underlying memory-safety correction.
- **!19492 / !19494 — Scanned master/backport pair.** MQTT-SN flag corrections; protocol-specific.
- **!19493 — Scanned, merged.** Qt related-packet delegate placement correction; UI-specific.
- **!19499 / !19522 — Scanned, merged.** WSLua/UAT preference lifecycle work; useful corroboration for correct UAT ownership/unload behavior.
- Remaining MRs in the exact set were dependency/package updates, automatic data/translation updates, small leak/null/warning cleanups, protocol registry/value changes, UI maintenance, or refactors without additional durable reviewer guidance. They were inspected and count as reviewed; no notebook rule was manufactured from low-signal evidence.

## Notebook changes

`dissector-conventions.md` was updated with the accepted !19496 dispatch-ordering rule. The convention update was committed separately before this ledger so the ledger records the completed run state.