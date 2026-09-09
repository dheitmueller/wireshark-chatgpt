# Automated MR review ledger: !17571–!17620

Reviewed 2026-09-09 using GPT-5.6 Sol.

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Exactly 50 previously unreviewed merge requests were reviewed in this run. Merged MRs were treated as stronger evidence than closed or superseded submissions, and maintainer-authored/reviewed changes—especially Guy Harris guidance—were weighted accordingly.

| MR | Outcome | Depth | Notes |
|---|---|---|---|
| !17571 | merged | Scanned | `Prep for 4.2.8`; release-note preparation only, no durable new review convention. |
| !17572 | merged | Deep | `PROXY: Use a CONVERSATION_PROXY type`; dedicated conversation identity prevents collisions/recursive dispatch when a shim/proxy changes conversation semantics. Promoted to architecture notes. |
| !17573 | merged | Scanned | `Qt: Allow saving files with Qt 6.8`; Qt compatibility adjustment, no broader convention promoted. |
| !17574 | merged | Scanned | `PFCP: add new Nokia vendor-specific IEs`; protocol-specific extension without reusable review guidance. |
| !17575 | merged | Discussion-focused | `BLF: Fix warnings found by cppcheck`; John Thacker flagged trailing whitespace because commit checks reject it. Existing notebook already records whitespace pre-submit checks, so this is corroboration only. |
| !17576 | merged | Scanned | `Capture: don't retrieve both monitor mode and non monitor mode caps`; avoids stateful capability-probe side effects, but no new general rule promoted. |
| !17577 | merged | Scanned | Qt 6.8 save compatibility backport; duplicate implementation evidence from !17573. |
| !17578 | merged | Deep | `DHCPv4: Work around non RFC 3925 compliant Option 124 implementations`; bounds recovery to the enclosing option and reports nonconformance with expert info. Corroborates existing malformed-input guidance. |
| !17579 | merged | Deep | `packet-amr dissector improvements for BWE and OA`; introduced GCC-style `packed` use that broke Windows builds, providing negative portability evidence later corrected by !17583 and superseded architecturally by !17600. |
| !17580 | merged | Scanned | `bthci_cmd/evt: Bluetooth HCI update to spec v6.0`; large protocol-spec update, no durable human-review lesson extracted. |
| !17581 | merged | Scanned | `CMake: Require CMake 3.16 or later`; support-baseline update, no new convention beyond existing platform/build policy. |
| !17582 | merged | Scanned | `USB-CCID: Handle RDR_to_PC_Parameters message with dwLength 0`; guards zero-length protocol data. Corroborates existing length/empty-payload handling. |
| !17583 | merged | Deep | `amr-h: Remove attribute packed, it's not supported by all compilers.`; immediate portability correction to !17579. Promoted with !17592/!17600. |
| !17584 | merged | Scanned | `logray: add falco-libs version to info`; dependency-version reporting only. |
| !17585 | merged | Scanned | `Windows: Update falcosecurity-libs to 0.18.1`; dependency maintenance only. |
| !17586 | merged | Scanned | `macos-setup*.sh: Update falcosecurity-libs to 0.18.1`; pinned download checksum corroborates existing dependency-integrity practice; no new rule promoted. |
| !17587 | merged | Scanned | `Windows:Update to libssh-0.11.0-1 and lz4-1.10.0-1`; dependency maintenance only. |
| !17588 | merged | Scanned | `macos-setup.sh: Update to libssh-0.11.1 and lz4-1.10.0`; dependency maintenance only. |
| !17589 | merged | Discussion-focused | `NPCAP: Update to 1.80`; Gerald Combs requested the release notes also be updated. !17603 supplied that follow-up. Promoted to submission conventions. |
| !17590 | merged | Deep | `AppleTalk: Make sure we have valid addresses`; validates address type/length before direct data access and replaces hard-coded byte counts with `sizeof`. Promoted to dissector conventions. |
| !17591 | merged | Deep | `bt-dht: flag duplicate and unordered keys`; uses CHAT severity for common tolerated noncanonical behavior. Promoted as evidence that expert severity should track operational impact, not merely formal nonconformance. |
| !17592 | closed | Discussion-focused | `ws_attributes.h: Introduce WS_PACKED() macro`; Guy Harris questioned compiler coverage and, more importantly, said structure overlay was a candidate for arrays instead. Closed/unmerged, so weighted below merged evidence; Guy's recommendation is strongly corroborated by his merged !17600 implementation. |
| !17593 | merged | Scanned | AppleTalk address-validation release-4.4 backport; duplicate of !17590. |
| !17594 | merged | Scanned | AppleTalk address-validation release-4.2 backport; duplicate of !17590. |
| !17595 | merged | Scanned | `radiotap: Update with link to document`; documentation maintenance only. |
| !17596 | merged | Deep | `RELOAD Framing: Make sure we have valid addresses`; validates address data and moves temporary key allocation into wmem file scope. Follow-up !17606 catches the allocator-family mismatch. |
| !17597 | merged | Scanned | `PortableApps: Add the Donation page`; packaging metadata only. |
| !17598 | merged | Scanned | RELOAD framing address-validation release-4.4 backport; duplicate of !17596. |
| !17599 | merged | Scanned | RELOAD framing address-validation release-4.2 backport; duplicate of !17596. |
| !17600 | merged | Deep / high-authority | `RF4CE: construct octet strings in arrays, not structures.`; authored and merged by Guy Harris. Replaces native-structure wire overlays with explicit byte arrays/copies and strongly confirms the !17592 review direction. Promoted to dissector/build conventions. |
| !17601 | merged | Scanned | `Qt: introduce sampling options for the RTT graph`; substantial feature but no substantive human-review comments in the corpus snapshot. |
| !17602 | merged | Scanned | `ieee1905: Remove Category from Proxied Encap DPP`; corrects a spec misinterpretation and cites the exact specification section; protocol-specific evidence only. |
| !17603 | merged | Discussion-follow-up | `Mention NPCAP update.`; implements Gerald Combs's !17589 request by documenting the bundled Npcap 1.80 upgrade in release notes. |
| !17604 | merged | Scanned | PortableApps donation-page release-4.4 backport; duplicate of !17597. |
| !17605 | merged | Scanned | PortableApps donation-page release-4.2 backport; duplicate of !17597. |
| !17606 | merged | Deep | `RELOAD Framing: Call the matching wmem_free scope`; fixes `wmem_alloc(file_scope)` followed by `g_free`. Gerald Combs suggested packet-pool allocation so explicit free is unnecessary when lifetime permits. Promoted to dissector conventions. |
| !17607 | merged | Scanned | Matching-wmem-free release-4.4 backport; duplicate of !17606. |
| !17608 | merged | Scanned | Matching-wmem-free release-4.2 backport; duplicate of !17606. |
| !17609 | merged | Scanned | `ORAN FH CUS: Make ST8 'ready' field more visible`; adds sanity/expert checks, no new cross-cutting review rule. |
| !17610 | merged | Scanned | `Build: 4.4.1`; release mechanics/security release-note aggregation only. |
| !17611 | merged | Scanned | `Build: 4.2.8`; release mechanics/security release-note aggregation only. |
| !17612 | merged | Deep | `DOF: Use a flexible array`; replaces a one-element trailing-array idiom with a standard flexible array to improve correctness/static-analysis clarity. Promoted to coding/dissector conventions. |
| !17613 | merged | Scanned | `PTP: Fix analysis when sequenceId resets happen.`; separates timing thresholds for related vs consecutive messages. Protocol-analysis fix, no new general convention promoted. |
| !17614 | merged | Scanned | `USB-CCID: Handle RDR_to_PC_Parameters message with dwLength 0`; release-4.4 backport of !17582. |
| !17615 | merged | Scanned | `Wi-SUN: Fix & Update Node Role IE`; protocol-field correction, no durable human-review lesson extracted. |
| !17616 | merged | Scanned | `Version: 4.4.1 → 4.4.2`; release-cycle reset/version maintenance only. |
| !17617 | merged | Scanned | `Version: 4.2.8 → 4.2.9`; release-cycle reset/version maintenance only. |
| !17618 | merged | Scanned | `Release notes: Mark a couple of issues as fixed`; release-note bookkeeping only. |
| !17619 | merged | Deep / high-authority | `Clean up message boxes.`; authored and merged by Guy Harris. Uses primary + informative text instead of detailed text for information that must appear consistently across macOS, Windows, and X11/Wayland. Promoted to platform/GUI conventions. |
| !17620 | merged | Scanned | `EAP: Don't logically OR encodings`; removes redundant/incorrect logical-OR expression. Straightforward correctness cleanup, no broader convention needed. |

## Durable findings promoted

- A proxy/shim layer that changes conversation lookup/dispatch semantics should use a distinct conversation type rather than reusing an underlying transport identity (!17572).
- Do not model on-wire octet strings by overlaying compiler-packed native structs. Prefer explicit byte arrays and copies; this is both ABI-portable and easier to reason about (!17579, !17583, !17592, especially merged Guy Harris MR !17600).
- When directly consuming `pinfo` address data, validate the expected address type/data/length first and prefer `sizeof` over magic byte counts (!17590, corroborated by !17596).
- Match wmem allocation/free families and scopes. Where lifetime permits, packet-scope/pool allocation can remove unnecessary manual frees (!17596, !17606; Gerald Combs review).
- Use standard C flexible-array members instead of one-element trailing arrays for variable-sized objects (!17612).
- Expert-info severity should reflect real operational/interoperability impact; common tolerated noncanonical input need not be escalated merely because it violates canonical ordering (!17591).
- User-visible bundled dependency changes belong in release notes (!17589, !17603; Gerald Combs review).
- For cross-platform Qt message boxes, put secondary explanatory text in the informative-text field when it must be visible consistently; detailed text has platform-specific presentation semantics (!17619, Guy Harris).
