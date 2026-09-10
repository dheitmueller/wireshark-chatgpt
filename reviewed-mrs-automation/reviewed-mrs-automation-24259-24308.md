# Wireshark MR review batch !24259-!24308

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed MR set from `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, and the available per-run files under `reviewed-mrs-automation/`, using individual MR entries rather than assuming filename ranges imply coverage. The historical !17571-!17620 batch remains included in the reviewed set. No previously reviewed !242xx entries were found, so the fifty highest-numbered unreviewed corpus MRs were !24308 down through !24259 inclusive.

Exactly 50 MRs were reviewed in this run.

| MR | Review | Notes |
|---|---|---|
| !24308 | Deep | Guy Harris-authored merged checked-add fix; strong corroboration of existing hostile-offset arithmetic guidance. |
| !24307 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for btatt: make dissect_handle() return value as out param; no additional durable convention promoted. |
| !24306 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for SMB: Enable dissecting of NEGOTIATE response for Windows for Workgroups 3.1a; no additional durable convention promoted. |
| !24305 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for pcapng-sysdig: Tweak the error message; no additional durable convention promoted. |
| !24304 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for snort config: Fix leak; no additional durable convention promoted. |
| !24303 | Deep | Merged first-contribution UBX NAV-RELPOSNED dissector; diff and review checked; no new general convention. |
| !24302 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for GitLab CI: Build Rocky/RHEL 10 and SUSE 16 packages; no additional durable convention promoted. |
| !24301 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for QUIC: Tighten heuristic by strengthening draft version check; no additional durable convention promoted. |
| !24300 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for QUIC: Tighten heuristic by strengthening draft version check; no additional durable convention promoted. |
| !24299 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for Qt+CMake: Fix build without Qt Multimedia; no additional durable convention promoted. |
| !24298 | Deep | Merged JSON wiretap lifetime/string-boundary fix. John Thacker noted that a length-delimited token should be copied/NUL-terminated before passing to a C-string timestamp parser; promoted. |
| !24297 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for Qt: Enable smaller sizes for the MainWindow; no additional durable convention promoted. |
| !24296 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for Qt: Move capture interface selection to own widget; no additional durable convention promoted. |
| !24295 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for Qt: Create stylesheets for LearnCardWidget; no additional durable convention promoted. |
| !24294 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for VoIP Calls: Set SIP call state back to setup when retry occurs; no additional durable convention promoted. |
| !24293 | Deep | Merged conversion of a vendor-OUI switch into a dissector table; useful extension-point exemplar, but existing notebook already prefers normal dissector-table mechanisms. |
| !24292 | Deep | Merged RTPS truncation fix; preserves available partial bytes and exits the loop when packet-controlled member length exceeds remaining data. Reinforces existing truncation/bounds guidance. |
| !24291 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for RTPS: Dissect PID_IDENTITY_TOKEN and PID_PERMISSIONS_TOKEN security tokens; no additional durable convention promoted. |
| !24290 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for QUIC: Tighten heuristic by strengthening draft version check; no additional durable convention promoted. |
| !24289 | Deep | Merged RTITCP reassembly fix uses the protocol's full 30-bit length consistently for PDU framing, reported length, and tree length; includes focused capture evidence. Reinforces framing/test guidance. |
| !24288 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for ieee80211: Add dissector for Huawei vendor specific IE; no additional durable convention promoted. |
| !24287 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for Enabled protocols: Update WSUG image and dialog description; no additional durable convention promoted. |
| !24286 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for MySQL: Use character_set_client for Bulk Execute Stmt parameters; no additional durable convention promoted. |
| !24285 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for MySQL: Use character_set_client for Bulk Execute Stmt parameters; no additional durable convention promoted. |
| !24284 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for SSH: add support for sntrup761x25519-sha512@openssh.com key exchange method; no additional durable convention promoted. |
| !24283 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for Qt: Welcome Banner implementation; no additional durable convention promoted. |
| !24282 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for oran_tap: Show UL beamIds in row output; no additional durable convention promoted. |
| !24281 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for OpenVPN: Fix Wrapped Client Key fields; no additional durable convention promoted. |
| !24280 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for ieee80211: Add dissector for Telecom Infra Project vendor specific IE; no additional durable convention promoted. |
| !24279 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for MySQL: Use character_set_client for Bulk Execute Stmt parameters; no additional durable convention promoted. |
| !24278 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for IEEE 802.11: Add NULL check; no additional durable convention promoted. |
| !24277 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for Qt: Backport sidebar changes to 4.6; no additional durable convention promoted. |
| !24276 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for HTTP: Check strstr() return before incrementing and dereferencing; no additional durable convention promoted. |
| !24275 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for HTTP: Check strstr() return before incrementing and dereferencing; no additional durable convention promoted. |
| !24274 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for OpenVPN: Fix Wrapped Client Key fields; no additional durable convention promoted. |
| !24273 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for SMB2: Fix some endian issues on fields; no additional durable convention promoted. |
| !24272 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for Qt: Fix InfoBanner warning; no additional durable convention promoted. |
| !24271 | Deep | Merged commit-message template cleanup makes AI-Assisted declaration syntax explicit/consistent; submission-template maintenance, no broader new rule. |
| !24270 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for ISO 8583: Use a wmem_strbuf_t; no additional durable convention promoted. |
| !24269 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for ISO 8583: Use a wmem_strbuf_t; no additional durable convention promoted. |
| !24268 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for HTTP: Check strstr() return before incrementing and dereferencing; no additional durable convention promoted. |
| !24267 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for Qt: Move content of sidebar widgets to JSON; no additional durable convention promoted. |
| !24266 | Deep | John Thacker-authored release-4.4 backport: parse signed and unsigned Snort parameters in their actual domains with range checks rather than casting between them; corroborates semantic-type guidance. |
| !24265 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for ISO 8583: Use a wmem_strbuf_t; no additional durable convention promoted. |
| !24264 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for Qt: Add check for development version during runtime; no additional durable convention promoted. |
| !24263 | Deep | Merged CMake JSON translation generator with explicit custom-command inputs/dependencies; consistent with existing generated-output build-graph guidance. |
| !24262 | Deep | John Thacker-authored release-4.6 backport of signed/unsigned Snort parser fix; corroborates !24266. |
| !24261 | Deep | Merged nested-TLS fix with substantial John Thacker review. Generic current-layer numbering conflated desegmentation reinvocation with semantic nesting; accepted design uses explicit per-protocol depth and distinct lookup/get-or-create session semantics. Promoted. |
| !24260 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for CIP dissector: Adding more vendors; no additional durable convention promoted. |
| !24259 | Scanned | Scanned corpus metadata, discussion presence, and change purpose for CIP dissector: Fixed size calculation for STRINGN; no additional durable convention promoted. |

## Durable findings promoted

- **!24261 — semantic protocol depth is not the same as dissector invocation/layer order.** John Thacker found that `pinfo->curr_proto_layer_num` could not distinguish repeated TLS dissector invocations caused by TCP desegmentation from true nested TLS. The merged solution moved nesting identity to explicit per-protocol depth (`p_set_proto_depth`) and maintained separate TLS session state by semantic nesting level. The review also separated non-mutating session lookup from get-or-create behavior.
- **!24298 — isolate length-delimited tokens before C-string parsing.** John Thacker pointed out that passing the rest of the JSON buffer to `iso8601_to_nstime()` could let parsing continue beyond the token boundary and produce false results. The accepted fix uses token-bounded/NUL-terminated storage before string parsing.

## Strong corroborating evidence

- **!24308** reinforces checked arithmetic for packet-derived offset calculation.
- **!24292** reinforces validating packet-controlled member lengths against remaining TVB data while preserving visible partial bytes.
- **!24289** reinforces using the protocol's authoritative full-width length consistently across TCP PDU framing, TVB reported length, and protocol-tree item length; the contributor supplied a focused pcap demonstrating before/after reassembly.
- **!24262/!24266** reinforce parsing signed and unsigned configuration values according to their semantic domains, with range checking instead of cast-based coercion.
- **!24263** reinforces explicit build dependencies for generated outputs.
- **!24293** reinforces using dissector tables as standard extensibility points instead of growing private switch statements.

Closed, abandoned, or superseded work (where encountered in the snapshot) was not used as stronger architectural evidence than merged work.
