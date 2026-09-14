# Wireshark MR review automation ledger — !20139-!20090

Corpus commit reviewed: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

This run reviewed exactly 50 previously unreviewed merge requests. The already-reviewed set was reconstructed from the available per-run files in `reviewed-mrs-automation/` and the aggregate tracking where applicable, using exact MR IDs rather than assuming numeric intervals. The historical !17571-!17620 batch remains preserved and counted. The corpus commit is unchanged from the prior run, so the previously discovered !22966 hole remains accounted for and no newly populated higher-numbered hole outranked this frontier.

## Exact MRs reviewed

`!20139 !20138 !20137 !20136 !20135 !20134 !20133 !20132 !20131 !20130 !20129 !20128 !20127 !20126 !20125 !20124 !20123 !20122 !20121 !20120 !20119 !20118 !20117 !20116 !20115 !20114 !20113 !20112 !20111 !20110 !20109 !20108 !20107 !20106 !20105 !20104 !20103 !20102 !20101 !20100 !20099 !20098 !20097 !20096 !20095 !20094 !20093 !20092 !20091 !20090`

Count: **50**.

## Review notes

| MR | Status | Review result |
|---|---|---|
| !20139 | Scanned | Merged automatic release/data update for 4.2; maintenance only. |
| !20138 | Scanned | Merged automatic release/data update for 4.4; maintenance only. |
| !20137 | Scanned | Merged John Thacker SSH support for the `none` cipher plus defensive early return for unsupported MAC identifiers. Protocol-specific, with useful defensive API handling but no separate notebook rule. |
| !20136 | Deep | Merged Guy Harris pcapng architecture cleanup: common code validates generic block framing and passes handlers semantic block-content length rather than total block length. Promoted to `pcapng-extension-architecture-conventions.md`. |
| !20135 | Scanned | Merged John Thacker SSH cleanup replaces duplicated checksum presentation with `proto_tree_add_checksum_bytes`; corroborates preference for common helpers. |
| !20134 | Scanned | Merged Guy Harris pcapng indentation/cast cleanup; no new convention. |
| !20133 | Scanned | Merged FT_STRING encoding cleanup removing inappropriate `ENC_NA`; corroborates existing encoding-domain guidance. |
| !20132 | Deep | Merged Guy Harris pcapng change allowing low-level option handlers to update section state, completing the generic extension-state contract introduced by !20120. Promoted with that series. |
| !20131 | Deep | Merged Guy Harris cleanup removes unused Netflix-specific API and makes registration-only callbacks static. Promoted as post-genericization API-surface cleanup. |
| !20130 | Scanned | Merged Windows package update to minizip 2.0.9 and bundled dependencies; packaging maintenance. |
| !20129 | Scanned | Merged Windows CMake fix to copy liblzma required by minizip-ng after vcpkg dependency changes; packaging maintenance. |
| !20128 | Scanned | Merged Windows libgcrypt update to 1.11.1; dependency maintenance. |
| !20127 | Scanned | Merged Npcap 1.82 update; dependency/release maintenance. |
| !20126 | Scanned | Merged release-branch Npcap 1.80 backport; maintenance only. |
| !20125 | Scanned | Merged Qt wording/pluralization cleanup; UI/i18n maintenance. |
| !20124 | Scanned | Merged spelling cleanup; documentation/text maintenance. |
| !20123 | Scanned | Merged Guy Harris checker exception allowing packet-bblog.c to register frame.bblog fields when invoked by frame option dissection; tooling integration detail. |
| !20122 | Deep | Merged Guy Harris removal of Netflix BBLog-specific representation from common Wiretap structures/headers after generic extension state became available. Promoted with !20120/!20132. |
| !20121 | Scanned | Merged vcpkg-export dependency update; build/dependency maintenance. |
| !20120 | Deep | Merged Guy Harris generic per-section private state keyed by PEN/local block type, replacing Netflix-specific fields in `section_info_t`. Promoted to `pcapng-extension-architecture-conventions.md`. |
| !20119 | Scanned | Merged FT_STRING encoding checks/removal of `ENC_NA`; corroboration only. |
| !20118 | Scanned | Merged GTPv2 presentation cleanup removing double spaces; no durable architecture lesson. |
| !20117 | Deep | Merged John Thacker SSH cleanup limits unsupported-KEX diagnostics to the first pass. Added as redissection-aware diagnostic guidance. |
| !20116 | Scanned | Merged FT_STRING encoding cleanup; corroboration only. |
| !20115 | Deep | Merged Guy Harris pcapng NRB cleanup uses a single remaining-byte invariant, precomputes padding, and shares common padding consumption. Promoted to `bounded-container-parser-conventions.md`. |
| !20114 | Deep | Merged John Thacker SSH encrypted-fragment/truncation support designed so a truncated capture does not unnecessarily prevent later packet decryption. Promoted to `bounded-container-parser-conventions.md`. |
| !20113 | Deep | Merged Guy Harris pcapng cleanup replaces local padding aliases with canonical `WS_ROUNDUP_n()`/`WS_PADDING_TO_n()` helpers and unsigned byte counts. Promoted as corroborating pcapng alignment guidance. |
| !20112 | Scanned | Merged Guy Harris return-type correction makes `pcap_get_phdr_size()` unsigned because the domain is nonnegative; corroborates existing type-domain guidance. |
| !20111 | Scanned | Merged WiMax XML parser migration from Flex to libxml2 plus dead-field cleanup. Useful simplification, but existing notebook parser/refactoring guidance already covers the pattern. |
| !20110 | Deep | Merged Guy Harris replacement of more hand-rolled padding with canonical alignment helpers and additional unsigned size variables. Promoted with !20113/!20096. |
| !20109 | Scanned | Merged BACnet compiler-warning fix for potentially uninitialized `npdu_length`; warning-driven correctness repair. |
| !20108 | Scanned | Merged John Thacker SSH cleanup consolidates duplicate packet-info creation into one accurately named static helper; corroborates naming and deduplication guidance. |
| !20107 | Discussion-focused (open) | Window-title/session behavior proposal remains open and unmerged; deliberately down-weighted as non-accepted implementation evidence. |
| !20106 | Discussion-focused (closed) | GitLab-fork restoration documentation proposal closed unmerged; deliberately down-weighted. |
| !20105 | Scanned | Merged README.dissector documentation for Exported PDUs; useful contributor documentation but not a new engineering convention. |
| !20104 | Scanned | Merged MIKEY MIME Base64 decoding fix; protocol/application-specific. |
| !20103 | Scanned | Merged John Thacker IEEE 802.11 ANQP Neighbor Report correction follows the 2020 specification where older wording was structurally ambiguous and reportedly undeployed. Good standards-version evidence, but no new generic rule beyond current-spec validation guidance. |
| !20102 | Deep | Merged Guy Harris removal of unused Netflix-specific Wiretap helper left after genericization. Promoted with !20131. |
| !20101 | Scanned | Version bump 4.2.12 to 4.2.13; release bookkeeping. |
| !20100 | Scanned | Version bump 4.4.7 to 4.4.8; release bookkeeping. |
| !20099 | Scanned | Merged plugin_if documentation corrected to match actual function arguments; API documentation maintenance. |
| !20098 | Scanned | Merged checker change avoids fetching the modelines page while validating dissector links; tooling maintenance. |
| !20097 | Scanned | Merged O-RAN preference for displaying unscaled values; protocol/presentation-specific. |
| !20096 | Deep | Merged Guy Harris pcapng padding cleanup reuses previously computed SPB padding and canonical `WS_ROUNDUP_4()` for options. Promoted as corroboration. |
| !20095 | Scanned | Merged O-RAN state-access hardening around `ack_nack_requests`; implementation-specific concurrency/state caution, no separate rule promoted. |
| !20094 | Scanned | Build/release 4.2.12 bookkeeping. |
| !20093 | Scanned | Build/release 4.4.7 bookkeeping. |
| !20092 | Scanned | Merged GitLab CI path corrections; CI maintenance. |
| !20091 | Deep | Merged Guy Harris SMB extended-attribute list fix: list length includes its own size field, minimum size must be validated, and nested entries must remain within the declared list boundary. Promoted to `bounded-container-parser-conventions.md`. |
| !20090 | Scanned | Merged GitLab CI macOS workaround cleanup/replacement; CI maintenance. |

## Durable notebook changes from this run

Created `pcapng-extension-architecture-conventions.md` with four accepted patterns:

1. Normalize and validate common block framing before dispatch, and pass subtype handlers semantic content length (!20136).
2. Keep extension-private per-section state behind a generic keyed mechanism rather than adding vendor-specific fields to common structures (!20120, !20122, !20132).
3. Remove obsolete extension APIs and staticize registration-only callbacks after genericization (!20131, !20102).
4. Prefer canonical alignment helpers and compute padding once where practical (!20113, !20110, !20096).

Created `bounded-container-parser-conventions.md` with three primary parser/state rules plus one corroborating redissection rule:

1. Declared aggregate/list lengths are hard parser boundaries and must include/validate their own mandatory headers where specified (!20091).
2. Prefer a monotonic remaining-byte invariant for bounded variable-record loops (!20115).
3. Treat capture truncation/fragmentation separately from protocol failure so stateful dissection can recover when the cipher/framing permits (!20114).
4. Avoid repeating first-pass-only diagnostics during redissection (!20117).

## Selection continuity

After this run, the descending frontier is below **!20090**, subject to any higher-numbered corpus hole populated by a future corpus refresh. Future runs must continue reconstructing the reviewed set from exact individual ledger entries rather than assuming intervals are complete.