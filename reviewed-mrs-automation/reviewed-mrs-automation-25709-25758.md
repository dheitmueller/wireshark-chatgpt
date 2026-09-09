# Wireshark MR automation review: !25709–!25758

Reviewed on 2026-09-09, working from newest toward older MRs.

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`
- Selection rule: the 50 highest-numbered MRs in the corpus not already present in `reviewed-mrs.md` or any existing `reviewed-mrs-automation/` ledger.
- Exact reviewed set: every MR from **!25758 down through !25709**, inclusive (50 MRs).
- Merged MRs were treated as accepted evidence; open/abandoned/superseded material was downweighted. !25755 was open in this corpus snapshot and is therefore provisional only.

## Review status meanings

- **Deep** — relevant implementation diff and substantive design/review context were examined closely enough to extract or corroborate durable guidance.
- **Discussion-focused** — the human review/design discussion was the primary useful evidence; implementation was checked sufficiently to understand the discussion.
- **Scanned** — purpose, disposition/context, discussion presence, and relevant implementation were checked; no distinct durable notebook lesson justified deeper treatment.

| MR | Review | Notes |
|---|---|---|
| !25758 | Deep | RDM E1.37-4 FTC additions. New-standard implementation had no equipment implementation available, so the author supplied a spec-derived capture to exercise the awkward CRC16 calculation. Useful test-vector corroboration; no new standalone rule promoted. |
| !25757 | Scanned | sharkd I/O Graph error-path cleanup. Rollback of already-added graphs on later failure; same transactional cleanup family already represented elsewhere. |
| !25756 | Deep | sharkd tap registration cleanup. Refactors single-tap registration and removes/frees all earlier registrations when a later one fails; strong rollback/partial-initialization exemplar, already consistent with existing lifetime guidance. |
| !25755 | Discussion-focused (open snapshot) | WSLua Lua Language Server definition generation. Guy Harris clarified the role of LuaLS; author demonstrated IDE behavior and kept unsupported global-generation work out of scope. Open/unmerged, so not treated as accepted architecture. |
| !25754 | Deep | TLS/DTLS application dispatch now sets `pinfo->match_string` to negotiated ALPN and restores prior dispatch metadata after nested calls. Added dispatch-context convention. |
| !25753 | Scanned | `.mailmap` / AUTHORS attribution cleanup for contributor credit. No durable engineering convention beyond ordinary attribution maintenance. |
| !25752 | Scanned | Automatic data/translation update. No durable human-review lesson. |
| !25751 | Scanned | Automatic data/translation update. No durable human-review lesson. |
| !25750 | Scanned | Automatic data/translation update. No durable human-review lesson. |
| !25749 | Discussion-focused | Qt text-codec menu ranking replaces accidental regex/alphabetical heuristics with explicit category data. Guy Harris also clarified ambiguous “codec” terminology in the MR title. Useful explicit-data-over-incidental-ordering example, but not promoted as a broad Wireshark rule. |
| !25748 | Scanned | Broad dissector-warning cleanup by Martin Mathieson. Mostly mechanical type/constant/comment corrections; no additional convention beyond existing warning/static-check guidance. |
| !25747 | Scanned | Qt system translation-path handling follow-up. Platform/Qt-specific maintenance; no new durable rule. |
| !25746 | Scanned | Qt system translation-path handling. Platform/Qt-specific maintenance; no new durable rule. |
| !25745 | Scanned | pcapng-sysdig timestamp-presence state correction. Correctness fix to restore record metadata after setup ordering; no separate reusable convention promoted. |
| !25744 | Scanned | Stratoshark “View as File Format” registration repair. Restores required MIME encapsulation/table registration; no new architecture beyond using existing registration mechanisms. |
| !25743 | Scanned | Adds `ENC_TIME_WINDOWS` / FILETIME support to common time encoding. Shared-encoding cleanup, no new review lesson. |
| !25742 | Scanned | Adds FT_PROTOCOL PINO items to protocol layer list. Internal tree/layer accounting correction; no new convention. |
| !25741 | Deep | RDM manufacturer-specific PID dissector table, validated with both capture and Lua plugin. Added vendor-extension dispatch convention. |
| !25740 | Scanned | Adds missing `WTAP_ENCAP_PROCMON` table entry and wording cleanup. Registry maintenance; no new lesson. |
| !25739 | Deep | HTTP/2 multipart heuristic accepts valid close-delimiter with or without trailing CRLF. Anders Broman challenged the grammar interpretation; author walked the RFC grammar and enclosing DATA length to show both forms valid, after which the thread was resolved and MR merged. Strong reminder to validate heuristics against full grammar/context rather than one observed encoding. |
| !25738 | Deep | Fileshark ZIP support uses streaming parsing and deliberately avoids recursive archive dissection as ZIP-bomb mitigation. Martin Mathieson caught an hf type/length mismatch. Added bounded-container parsing convention. |
| !25737 | Scanned | BLE ATT write-command service/characteristic UUID recognition. Protocol-state lookup correction; no additional general convention. |
| !25736 | Scanned | Expands `SP_WARNING_MSG` comments to describe actual capture warning semantics. Documentation companion to !25732; no separate rule. |
| !25735 | Scanned | Supplies additional unregistered items for dissector warning/tooling cleanup. No distinct durable rule. |
| !25734 | Deep | Core `proto_tree_add_item_ret_varint()` now diagnoses/throws on zero-length malformed varints, allowing LTP/TCPCL to remove duplicate checks. Added shared-parser-contract convention. |
| !25733 | Scanned | Adds missing wiretap encapsulation mapping for ZWAVE_TAP. Registry correctness fix; no new lesson. |
| !25732 | Deep | Guy Harris-authored capture warning propagation keeps libpcap success-with-warning distinct from fatal errors across dumpcap/capture-session interfaces. Added high-confidence capture diagnostic convention. |
| !25731 | Scanned | Additional dissector warning cleanup. No new guidance beyond existing static/warning checks. |
| !25730 | Discussion-focused | DVB-S2 CNI mapping corrected to normative ETSI encoding rather than stale secondary SatLabs mapping. Alexis La Goutte asked to retain useful historical spec linkage. Reinforces checking normative specifications and preserving useful provenance; no new standalone file needed. |
| !25729 | Scanned | TCP duplicate-segment counting/table display correction. Stateful-analysis correctness fix; no new convention beyond existing state consistency guidance. |
| !25728 | Scanned | Release version bump 4.4.17→4.4.18. No engineering lesson. |
| !25727 | Scanned | Release version bump 4.6.7→4.6.8. No engineering lesson. |
| !25726 | Scanned | Build/release 4.4.17 maintenance. No engineering lesson. |
| !25725 | Scanned | Build/release 4.6.7 maintenance. No engineering lesson. |
| !25724 | Scanned | O-RAN FH CUS timing-header fallback matches a section to the latest prior entry when no exact match exists. Protocol-specific state correlation; no new general rule. |
| !25723 | Scanned | TCP duplicate-segment count/table display correction related to !25729. No additional lesson. |
| !25722 | Scanned | ciscodump caps packet-buffer writes at `PACKET_MAX_SIZE` to avoid heap overflow/truncation mismatch. Strong buffer-bound fix but already covered by existing capacity/length safety guidance. |
| !25721 | Scanned | Same ciscodump `PACKET_MAX_SIZE` hardening family as !25722. No additional lesson. |
| !25720 | Scanned | Prep for release 4.4.17. No engineering lesson. |
| !25719 | Scanned | Prep for release 4.6.7. No engineering lesson. |
| !25718 | Scanned | C2P protocol display name made greppable/consistent. Naming cleanup; no broader convention. |
| !25717 | Deep | BER unknown extra SEQUENCE/SET fields reclassified from `PI_MALFORMED` to `PI_UNDECODED` because parsing can safely continue. Added diagnostic-category convention. |
| !25716 | Deep | Extcap secret scrubbing gains `memset_explicit` and `explicit_bzero` feature detection in addition to Windows/`memset_s` paths. Added portable non-elidable zeroization convention. |
| !25715 | Scanned | UET TSS header-field correction for issue #21400. Protocol-specific layout fix; no new general convention. |
| !25714 | Scanned | O-RAN FH CUS registers two SE23 fields. Registration completeness fix; no new convention. |
| !25713 | Deep | Guy Harris-authored `cap_pipe_open_live()` cleanup returns `bool` directly instead of making callers infer failure through platform-specific state. Added capture status-contract convention. |
| !25712 | Scanned | Zebra uninitialized-memory/copy-paste fix (cherry-picked fix). Defensive correctness, no new distinct rule. |
| !25711 | Scanned | eDonkey protocol name corrected to documented spelling/case. Naming consistency only. |
| !25710 | Scanned | dumpcap stops passing a pointer to the single global loop-data object and references the global directly, making ownership/cardinality explicit. Local simplification, no new standalone rule. |
| !25709 | Deep | Extcap SSH parameters explicitly scrub credential-related strings before free; !25716 strengthens the portable zeroization implementation. Added extcap security convention. |

## Notebook changes promoted from this run

- `extcap-security-conventions.md`: scrub authentication material before free; prefer non-elidable zeroization primitives (!25709, !25716).
- `protocol-input-diagnostics-conventions.md`: distinguish undecoded from malformed input (!25717); put universal malformed-representation checks in shared parser helpers (!25734).
- `container-api-conventions.md`: bounded/streaming parsing and no automatic recursive archive expansion for untrusted container input (!25738).
- `dissector-dispatch-context-conventions.md`: match metadata must describe the actual selector and be save/set/call/restore scoped; vendor extension namespaces should use dissector tables (!25754, !25741).
- `capture-diagnostic-conventions.md`: preserve success-with-warning versus failure and return semantic success/failure directly from helpers (!25732, !25713).