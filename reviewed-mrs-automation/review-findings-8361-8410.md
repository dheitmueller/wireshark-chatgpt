# Review findings — !8361–!8410

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

Status: 48 merged; !8384 and !8367 closed/unmerged and down-weighted.

## Strongest findings

- !8362 (João Valverde): WSLua constant generation moved from a separate Python header parser to Wireshark's shared introspection/generated-enum metadata. Prefer one source of truth for constants exported to language bindings.
- !8368 (João Valverde): RPC mixed wire-byte coordinates with decoded UTF-8 coordinates. Keep protocol lengths in the wire representation and decoded-string operations in the decoded representation.
- !8369: Alexis La Goutte required the new RADIUS dictionary to be reflected in explicit installer manifests and asked that the other Windows packaging path be checked. Runtime inclusion and package inclusion are separate obligations.
- !8371: Alexis La Goutte's review caught duplicate fields, incorrect masked-Boolean metadata, an out-of-order extended value-string table, naming issues, and the lack of a sample capture. The contributor supplied a pcap and fixed the issues before merge.
- !8379 and !8402 (John Thacker): use encoding-aware TVBuff helpers instead of raw/manual string extraction.
- !8398, !8400, and !8394 (João Valverde): preserve useful compiler diagnostics while relaxing only their fatality, preferably at the narrowest scope.
- !8363 (Guy Harris): shell test syntax requires the closing bracket to be a separate token. High-authority but narrow portability evidence.
- Closed !8367 and !8384 are superseded by merged !8371 and !8373 respectively.

## Per-MR inventory

!8410 merged — NAS-5GS static value_string_ext cleanup.
!8409 merged — RTPS secure volatile DATA; review corrected writer-ID condition and noted GNU-only binary literals.
!8408 merged — NAS-5GS Extended CAG length handling.
!8407 merged — NR RRC v17.2.0 update.
!8406 merged — BPv7 avoids retaining metadata when bundle identity is absent.
!8405 merged — 4.0.1 release transition.
!8404 merged — centralized UTF-8 validation and debugging helpers.
!8403 merged — 4.0.0 release build transition.
!8402 merged — SMB dialect decoding through ENC_ASCII.
!8401 merged — release-note placement correction.
!8400 merged — Qt 6.4 deprecation cleanup and warning-demotion support.
!8399 merged — JSON 3GPP duplicate-code removal.
!8398 merged — deprecated Qt API remains warning, not fatal error.
!8397 merged — obsolete Qt workaround removed.
!8396 merged — LTE RRC v17.2.0 update.
!8395 merged — GTP ENC_APN_STR stable backport.
!8394 merged — stringop-overflow remains visible but non-fatal.
!8393 merged — Qt 6.4 deprecation fixes.
!8392 merged — LPP v17.2.0 update.
!8391 merged — TECMP preference for embedded Ethernet tree placement.
!8390 merged — register NR-RRC Reconfiguration Complete by name.
!8389 merged — Lua FT_STRINGZPAD stable backport.
!8388 merged — WSLua FieldInfo supports FT_STRINGZPAD.
!8387 merged — NAS-5GS v17.8.0 update.
!8386 merged — explicit UTF-8 conversion for Qt 6.4 QVariant compatibility.
!8385 merged — wslog help and fatal-filter cleanup.
!8384 closed — superseded ISAKMP Fortinet notification change.
!8383 merged — BGP authority-bit stable backport.
!8382 merged — BGP authority-bit stable backport.
!8381 merged — BGP authority-bit stable backport.
!8380 merged — release notes for UTF-8 API improvements and Qt 6.
!8379 merged — GTP APN decode via ENC_APN_STR.
!8378 merged — authoritative master BGP authority-bit correction with RFC provenance.
!8377 merged — CI changes for 3.4 end-of-support.
!8376 merged — falcodump help output correction.
!8375 merged — Qt Base64 copy option.
!8374 merged — falcodump adapts to libsinsp/plugin schema changes.
!8373 merged — accepted ISAKMP Fortinet notification decode.
!8372 merged — separate 3GPP JSON handling and public JSON header.
!8371 merged — protected EHT EML operation-mode support after extensive review.
!8370 merged — PortableApps documentation and 64-bit-only packaging updates.
!8369 merged — 5x9 RADIUS dictionary plus installer manifests.
!8368 merged — RPC avoids blind UTF-8 truncation.
!8367 closed — untested draft predecessor to !8371.
!8366 merged — PFCP v17.6.0 update.
!8365 merged — RTPS optional-member alignment fix.
!8364 merged — stable backport of macOS shell-test fix.
!8363 merged — Guy Harris master shell-test syntax fix.
!8362 merged — WSLua constants generated from introspection metadata.
!8361 merged — automatic generated-data and registry update.
