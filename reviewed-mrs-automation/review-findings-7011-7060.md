# Review findings !7011–!7060

Corpus: `ddcaa22b51c68f594e425a23388c3a2086813054`

| MR | Outcome | Finding |
|---|---|---|
| !7060 | merged | Explicit EPAN plugin post-init phase after protocol initialization. |
| !7059 | merged | Traffic-table Qt cleanup; no durable rule. |
| !7058 | merged | Fix optional no-MaxMindDB build after UI refactor. |
| !7057 | merged | Preserve captured bytes; Guy Harris defines caplen/len semantics and favors format-aware repair/reporting. |
| !7056 | merged | Warning cleanup only. |
| !7055 | merged | Detachable traffic tabs; UI-specific. |
| !7054 | merged | Export raw typed model values, not formatted display strings. |
| !7053 | merged | Traffic tables move to Qt model/view; optional-build coverage gap exposed. |
| !7052 | merged | CPUID documentation cleanup. |
| !7051 | merged | Guy Harris: gate x86 CPUID intrinsic by target architecture, not compiler alone. |
| !7050 | merged | Remove leaking default reference bound to heap QString; scope state to methods that need it. |
| !7049 | merged | Backport of save-path leak fix. |
| !7048 | merged | Free temporary filename after successful rename. |
| !7047 | merged | Do not g_free memory owned by a wmem scope. |
| !7046 | merged | CI follows packaging-target rename. |
| !7045 | merged | New USB linktypes coordinated with libpcap/linktype registry first. |
| !7044 | merged | Conversation identity moves to typed element lists; wildcard endpoint identity remains distinct. |
| !7043 | merged | Save-with-Copy preserves Wiretap parser state and only reopens descriptors/path. |
| !7042 | merged | Remove unused conversation API option. |
| !7041 | closed | Translation files are managed via the external translation workflow, not hand-edited. |
| !7040 | merged | Automatic data/translation update. |
| !7039 | merged | Automatic data/translation update. |
| !7038 | merged | Automatic data/translation update. |
| !7037 | merged | Avoid low-value TCP reassembly text overwriting useful higher-layer INFO. |
| !7036 | merged | Revisit can retrieve existing MSP state without recreating first-pass state. |
| !7035 | merged | Restore TCP endpoint state between sibling upper-layer PDUs. |
| !7034 | merged | Internal-linkage cleanup. |
| !7033 | merged | OOO desegmentation offsets belong to logical reassembled stream, not current segment. |
| !7032 | merged | Backport of Touchlink filter typo fix. |
| !7031 | merged | Backport of Touchlink filter typo fix. |
| !7030 | merged | Narrowly suppress harmless warning in imported lrexlib code instead of diverging source. |
| !7029 | merged | Debian ABI symbol bookkeeping. |
| !7028 | merged | wmem_map does not copy keys; retain copied keys for the entry lifetime. |
| !7027 | merged | CIP documentation/common declaration cleanup. |
| !7026 | closed | Accidental Lua 5.3 MR; not precedent. |
| !7025 | merged | iWARP fragmented Send reassembly. |
| !7024 | closed | Dependency-major migration must cover packaging, setup scripts, CI/builders, and release branches. |
| !7023 | merged | O-RAN offset/count correctness fix. |
| !7022 | merged | Windows is not synonymous with MSVC; gate workaround on actual toolchain. |
| !7021 | merged | Packaging target rename/addition updates docs and CI together. |
| !7020 | merged | Weak nonheuristic dissector on nonstandard ports is disabled by default; generator updated too. |
| !7019 | merged | Pass resolved CMake executable to helper script instead of assuming PATH. |
| !7018 | merged | Conversation key retained by core must not live on caller stack. |
| !7017 | merged | Lazily allocate per-conversation dissector trees. |
| !7016 | merged | ASN.1 documentation version update. |
| !7015 | closed | CI-fatal warning caught by Alexis La Goutte; closed work not precedent. |
| !7014 | merged | Doxygen parameter-name cleanup. |
| !7013 | merged | Alexis La Goutte: split bugfix from feature so fix can be backported independently. |
| !7012 | merged | Debug UI adapts to typed conversation element tables. |
| !7011 | merged | Keep common by-ID API at guint32; use full element-key API for richer identities. |
