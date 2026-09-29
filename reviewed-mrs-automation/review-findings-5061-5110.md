# Wireshark MR review findings 5061-5110

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

| MR | Outcome | Finding |
|---|---|---|
| !5110 | Merged, master | ETI/XTI/EOBI generated dissectors. Anders Broman requested that the Python generator be kept in `tools/` so future regeneration remains maintainable; the accepted revision includes `tools/eti2wireshark.py`. Strong generated-source provenance/reproducibility evidence. |
| !5109 | Merged, master | Ubuntu test workaround unsets `XDG_CONFIG_HOME`. Uli Heilmeier asked for one MR per logical change and a commit message carrying the bug reference; the contributor split unrelated pytest work. The CI-local environment workaround is later superseded by the shared-fixture solution in !5129/!5134/!5152. |
| !5108 | Merged, release-3.6 | Stable backport of the BBLog field/bitmask improvement represented by master !5079; corroborating evidence only. |
| !5107 | Merged, master | ByteView hover preference is written back to the profile's recent state and documentation is corrected. Focused UI persistence fix; no broader convention extracted. |
| !5106 | Merged, release-3.6 | Backport of !5105's generated display-filter syntax correction. |
| !5105 | Merged, master | VoIP filter generator is updated for the display-filter grammar's comma-separated set syntax. Corroborates auditing UI/code generators when a user-facing grammar changes. |
| !5104 | Merged, master | Display-filter debug output is reduced and regex compile failures get clearer context. Logging/diagnostic cleanup; no new standalone rule. |
| !5103 | Merged, master | GitHub PR lockdown text links directly to the authoritative GitLab repository. Contributor-routing UX, not a coding convention. |
| !5102 | Merged, master | Updates c-ares project/download URLs consistently across docs, setup tooling, and CMake metadata. Routine dependency maintenance. |
| !5101 | Merged, master | ANSI MAP/TCAP fixes wrong header-field identities and disables unused registrations in both ASN.1 templates and generated sources. Corroborates keeping generator/template sources and generated output synchronized. |
| !5100 | Merged, release-3.6 | Stable packaging/CI update for Qt 5.15.3 and minimum macOS 10.13. Release corroboration only. |
| !5099 | Merged, master | Raises the macOS Intel CI deployment target to 10.13. Platform support maintenance. |
| !5098 | Merged, master | Moves macOS Intel CI package build to Qt 5.15.3. Dependency/toolchain maintenance. |
| !5097 | Merged, master | Pascal Quantin adds PCRE2 to both NSIS and WiX runtime DLL inventories. Corroborates keeping parallel installer dependency inventories consistent. |
| !5096 | Merged, master-3.2 | Release-note preparation/security issue enumeration for 3.2.18. No new coding convention. |
| !5095 | Merged, release-3.4 | Release-note preparation/security issue enumeration for 3.4.10. No new coding convention. |
| !5094 | Closed/unmerged | WIP TCP “suspicious packet” analysis. John Thacker flags 32-bit sequence wraparound after long transfers; author closes because the classification problem is broader than one MR. Lower-weight caution only; later accepted wrap-aware sequence guidance remains authoritative. |
| !5093 | Merged, master | macOS bundle-script comment cleanup and a pointer to CMake `fixup_bundle`. Documentation-only maintenance. |
| !5092 | Merged, release-3.6 | Stable backport of the PDCP-LTE registration-function naming fix from !5087. |
| !5091 | Merged, master | Restores early capture-filter-list loading so CLI `-f "predef:..."` works. Configuration initialization must precede consumers, but no new general rule was needed. |
| !5090 | Merged, master | CMake build-status cleanup. Graham Bloice explicitly notes that `CMAKE_BUILD_TYPE` is not the active configuration for multi-config generators such as Visual Studio. Strong corroboration of later !5327/!5344 multi-config fixes. |
| !5089 | Merged, master | TCAP/CAMEL transaction/SRT analysis is always maintained while presentation/stat output remains conditional. Useful separation of analysis state from display preference, but protocol-specific. |
| !5088 | Merged, master | Adds an MSYS2 dependency setup script. Build-environment convenience; no distinct durable rule. |
| !5087 | Merged, master | Renames PDCP-LTE registration function to the protocol-qualified name. Straightforward symbol correctness. |
| !5086 | Merged, master | Moves “message level omits source location” behavior into the logging macro contract so lower-level `ws_log_full()` keeps its full semantics. API-layer cleanup. |
| !5085 | Merged, master | Regex length APIs move from GLib `gssize` to C/POSIX `ssize_t` with configure detection and internal `config.h` additions. Useful historical portability evidence, but later !5514/!5520 are more precise for Windows ABI-facing I/O types. |
| !5084 | Merged, master | macOS libpcre2 bundle workaround. Guy Harris's substantive comments are punctuation/comment consistency only; no technical policy extracted. |
| !5083 | Merged, master | Fixes a PCRE2 archive-name typo in the Windows setup script. Routine maintenance. |
| !5082 | Merged, master | Adds an Arch Linux dependency setup script. Routine build-environment support. |
| !5081 | Merged, master | Reduces display-filter debug verbosity and makes NULL syntax-node logging safe. Diagnostic cleanup. |
| !5080 | Merged, master | Adds debug diagnostics for unexpected PCRE2 match errors and centralizes error-message conversion. Narrow regex robustness improvement. |
| !5079 | Merged, master | Jaap Keuter asks that BBLog packed flags use `proto_tree_add_bitmask()`; accepted code replaces manual parent/subtree/child boilerplate for three flag containers. Strong dissector-field idiom. |
| !5078 | Merged, master | Renames return-on-invalid-argument macros to value-generic names, logs failures as programming errors, and adds zero-value checking. Defensive API cleanup. |
| !5077 | Merged, master | Moves PCRE2 bootstrap after CMake because PCRE2's build requires CMake. Straightforward build dependency ordering. |
| !5076 | Merged, master | Actually invokes the newly added PCRE2 installer from the macOS setup sequence. Routine integration fix. |
| !5075 | Merged, master | Automated registry/generated-data/translation update. No human-review convention extracted. |
| !5074 | Merged, release-3.6 | Automated registry/generated-data update. No new convention. |
| !5073 | Merged, release-3.4 | Automated registry/generated-data update. No new convention. |
| !5072 | Merged, master-3.2 | Automated registry/generated-data update. No new convention. |
| !5071 | Merged, master | Display-filter parser detects EOF explicitly and reports “Unexpected end of filter expression” instead of treating a missing token as a normal token. Focused diagnostic correctness. |
| !5070 | Merged, master | Display-filter compile API records caller context, initializes output deterministically, and improves logging/error handling. Useful diagnostics/API hygiene; no new standalone rule. |
| !5069 | Merged, master | Fixes GCC maybe-uninitialized warnings by establishing explicit defaults. Routine warning cleanup. |
| !5068 | Merged, master | Restores display-filter debug syntax-tree representation. Debug tooling maintenance. |
| !5067 | Merged, master | Debian `libwiretap-dev` now depends on `libwsutil-dev` because its public headers require wsutil headers. Corroborates dependency-closed development packages/public header chains. |
| !5066 | Merged, master | Addresses PVS-Studio findings and documents intentional cases. Static-analysis cleanup; no additional convention beyond existing warning/checker guidance. |
| !5065 | Merged, master | MaxMind resolver closes child stderr only after successful spawn. A zero-initialized fd can be 0 (stdin), so partial-initialization cleanup must be gated on actual acquisition. This is the master origin of the rule later seen in stable !5133. Gerald Combs also states the normal master-first-then-backport workflow. |
| !5064 | Merged, release-3.6 | Backport of Guy Harris's !5063 external-plugin example fix; corroborates the public/private build boundary. |
| !5063 | Merged, master | Authored by Guy Harris. Removes Wireshark's private generated `config.h` from the example third-party plugin because external plugins cannot rely on that build-tree header. Very high-authority extension-boundary guidance. |
| !5062 | Merged, master | Adds PCRE2 to supported platform setup scripts, including macOS source build support. Dependency bootstrap maintenance. |
| !5061 | Merged, release-3.6 | Guy Harris removes obsolete `HAVE_CONFIG_H` conditionality and initially makes `config.h` unconditional in sample code. For the external plugin example this is immediately superseded by master !5063/!5064, which remove Wireshark's `config.h` entirely; retain !5061 only as evidence that Wireshark-owned build sources should not gate required config on an undefined Autoconf-era macro. |
