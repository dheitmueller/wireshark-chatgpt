# Review findings: Wireshark MRs !6711-!6760

Corpus commit ddcaa22b51c68f594e425a23388c3a2086813054. All 50 MRs were merged, so each is accepted implementation evidence; maintainer-authored changes and substantive maintainer review receive the strongest weight.

| MR | Outcome | Depth | Finding |
|---|---|---|---|
| !6760 | merged | Deep | Display-filter universal quantifiers. Joao Valverde modeled any/all as a semantic match qualifier across relation operators, with lexer, grammar, AST, VM, docs, release notes, and tests changed together. |
| !6759 | merged | Deep | Display-filter layer selectors. Adds total-layer and per-protocol occurrence bookkeeping to packet_info and field_info and tests nested IP; repeated-protocol identity must be protocol-relative. |
| !6758 | merged | Scanned | Visual Studio 2022 packaging/CMake backport; no new convention beyond existing build-baseline guidance. |
| !6757 | merged | Scanned | Broad Coverity cleanup including generated ASN.1 templates and initialized address/stat-table storage; corroborates existing initialization/generated-source guidance. |
| !6756 | merged | Scanned | IEEE 802.11 Clang Analyzer dead-store cleanup reviewed by Richard Sharpe; no new durable rule. |
| !6755 | merged | Deep | FPP NULL-conversation checks triggered substantive Gerald Combs/Dario Lombardo discussion. Invalid conversation use should be reported centrally as a dissector bug rather than silently accepted. |
| !6754 | merged | Scanned | Removes 32-bit Windows and CentOS 7 CI jobs while release notes record the support change; corroborates support-matrix/documentation coupling. |
| !6753 | merged | Scanned | Release-note notice for 32-bit Windows retirement; documentation-only corroboration. |
| !6752 | merged | Scanned | Moves Windows CI to Visual Studio 2022; no additional durable lesson. |
| !6751 | merged | Scanned | Defensive display-filter VM dump NULL check from Coverity; localized analyzer fix. |
| !6750 | merged | Scanned | Restores Arch pacman sysupgrade argument; setup-script correctness only. |
| !6749 | merged | Scanned | Additional FPP NULL checks around conversation add/delete paths; corroborates !6748/!6755. |
| !6748 | merged | Deep | Gerald Combs documents conversation proto-data API preconditions and makes add/get/delete report a dissector bug on NULL conversations. |
| !6747 | merged | Scanned | Interface-list resource leak fix; ordinary ownership cleanup. |
| !6746 | merged | Deep | John Thacker documents why faked proto items make parent/length mutation ambiguous and can distort hierarchy stats and performance. |
| !6745 | merged | Scanned | IPP switches to proto_tree_get_parent API rather than reaching into internals; reinforces abstraction-boundary use. |
| !6744 | merged | Scanned | NR RRC v16.8.0 generated/protocol update; no new reusable convention. |
| !6743 | merged | Scanned | LTE RRC v16.8.0 generated/protocol update; no new reusable convention. |
| !6742 | merged | Scanned | LPP v16.8.0 generated/protocol update; no new reusable convention. |
| !6741 | merged | Discussion-focused | Commit-message CI failure. Gerald showed the actual commit violated the 80-column rule; Alexis instructed amending and force-pushing the existing commit rather than replacing the MR. |
| !6740 | merged | Scanned | Registers additional XML MIME media types; straightforward dissector registration update. |
| !6739 | merged | Scanned | Reverts a Logwolf extcap directory split because the architecture should be solved differently; negative history but no standalone rule promoted. |
| !6738 | merged | Scanned | Documentation grammar correction; no engineering convention. |
| !6737 | merged | Scanned | Sparkle 2 support backport; dependency API migration corroboration. |
| !6736 | merged | Scanned | Sparkle 2 support backport on another branch; duplicate evidence. |
| !6735 | merged | Deep | John Thacker fixes hierarchy stats by using proto_registrar_is_protocol() rather than hfinfo->parent == -1; semantic registry type must beat structural/display heuristics. |
| !6734 | merged | Deep | PDCP-NR security state retains frame-aware key/configuration history and handles reestablishment boundaries; corroborates redissection-safe time-varying state rules. |
| !6733 | merged | Scanned | Setup scripts now handle no-argument invocation correctly; shell CLI robustness only. |
| !6732 | merged | Deep | Typed-item checker counts every real warning and drops a noisy consecutive-mask test. CI-facing diagnostics must affect checker status and unsound checks should be removed. |
| !6731 | merged | Scanned | Clang 14 CI backport; compiler baseline maintenance. |
| !6730 | merged | Scanned | Clang 14 CI migration; compiler baseline maintenance. |
| !6729 | merged | Scanned | Alpine/Arch setup scripts fail nonzero on errors, aligning installer-script failure semantics. |
| !6728 | merged | Scanned | RPM setup variable fix/backport; no broader lesson. |
| !6727 | merged | Scanned | RPM setup variable fix/backport; duplicate evidence. |
| !6726 | merged | Scanned | RPM setup variable fix; duplicate evidence. |
| !6725 | merged | Scanned | Automatic data update; no review convention. |
| !6724 | merged | Scanned | Automatic data/translation update; no review convention. |
| !6723 | merged | Scanned | Automatic data update; no review convention. |
| !6722 | merged | Deep | Large display-filter expression/function work with docs and tests. Review exposed a separate negative-time edge case, reinforcing type-specific edge testing for generic ftype operations. |
| !6721 | merged | Scanned | Sparkle 2 migration with local test note; no new general rule. |
| !6720 | merged | Scanned | Clang nonnull warning fixes in proto/ciscodump paths; localized nullability hardening. |
| !6719 | merged | Scanned | Display-filter VM dead-store cleanup; ordinary analyzer-driven cleanup. |
| !6718 | merged | Discussion-focused | QUIC ACK Frequency draft update reviewed by Ivan Nardi without a capture; merged spec update, but lack of representative sample limits testing evidence. |
| !6717 | merged | Scanned | IEEE1905 offset correction plus style review; protocol-specific fix. |
| !6716 | merged | Deep | IEEE 802.11 KDE work renamed a display-filter field and immediately broke tests; Richard Sharpe explicitly worried about user scripts. Filter abbreviations are compatibility-facing API. |
| !6715 | merged | Discussion-focused | OpenVPN tls-crypt support. Alexis requested a representative capture and the contributor supplied one; reinforces sample-capture expectations. |
| !6714 | merged | Scanned | DOCSIS sub-TLV dissection fix; backport question only. |
| !6713 | merged | Deep | Display-filter string scanner returns the token/error produced by the conversion helper instead of unconditionally returning STRING/CHARCONST. Lexer wrappers must propagate helper failure status. |
| !6712 | merged | Scanned | Adds debug logging identifying heuristic dissectors that claim a frame; useful diagnostics but no stronger new architecture rule. |
| !6711 | merged | Deep | Gerald Combs makes conversation-filter protocols dynamically registerable for plugins. Roland Knall review makes registration idempotent and centralizes priority ordering. |
