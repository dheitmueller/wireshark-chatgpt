# Durable conventions from Wireshark MRs 5111-5160

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly !5160 through !5111. Merged master changes are primary evidence; maintained-branch copies corroborate them. Closed !5141 and !5132 are lower-weight history only.

## Reusable protocol identifiers require historical state, not one mutable slot

Merged master !5151, authored by John Thacker, adds `wmem_multimap_t` because several protocols reuse transaction or fragment IDs during one capture. The container hashes on the semantic protocol key and stores multiple values in a frame-number-indexed tree, allowing nearest-prior lookup for the frame being dissected.

**Rule:** if a wire identifier can be reused, preserve generations/history and include capture position or another protocol-valid generation dimension in lookup. Redissection must recover state valid at the packet's point in history rather than the newest state learned later.

**API rule:** repeated map-plus-history patterns across dissectors justify a shared container abstraction. A new public wsutil container should be wired into public headers/build files, exported-symbol metadata, and project documentation; Jaap Keuter explicitly caught the documentation obligation here.

## Check the actual proto-data object, not a frame lifecycle proxy

Merged master !5153, authored by John Thacker, is the master origin of the Gryphon fix propagated through !5160 and later stable copies. `pinfo->fd->visited` can be true even when unusual TCP sequencing prevented Gryphon from executing on the nominal first pass.

**Rule:** before consuming dissector-owned proto-data, query for the state object itself. When creation is valid and idempotent, state existence is the correct initialization predicate; a frame's `visited` bit is not proof that every nested dissector ran.

## Put test-environment invariants in the shared fixture

Merged master !5129 removes `XDG_CONFIG_HOME` in Wireshark's common test environment because it has precedence over `HOME`. Stable !5134 propagates the fix. Merged !5152 removes the earlier GitHub Actions-only workaround once the fixture owns the invariant.

**Rule:** isolate command-line tests from host configuration at the shared subprocess environment boundary. Audit precedence variables, not just `HOME`, and remove runner-specific workarounds once the common fixture enforces the invariant everywhere.

## Separate a backportable correctness fix from new functionality

In merged master !5115, Jaap Keuter asks the contributor to split MKA Announcement padding correctness from new Announcement parsing so the fix can be backported without the feature. !5128 adds parsing on master, while !5154 and !5155 carry the focused correction to stable branches.

**Rule:** when a feature exposes an independently useful bug fix, make the correctness unit cherry-pickable on its own if supported branches may need it.

## Treat known custom-dissector helpers as compatibility contracts

Merged master !5135 changes `dissect_gtpv2_tai()` for 5GS TAC support. Pascal Quantin explicitly checks with Anders Broman because a known custom dissector calls the helper and offers an additive function instead if the signature change would be unacceptable. Anders accepts; !5137 and !5139 then backport the fix.

**Rule:** known external/custom dissector consumers matter even when a helper is not a formally versioned ABI. Before changing a shared helper signature, identify those consumers and decide whether an additive API is safer, especially before carrying the change to maintained branches.

## Follow specification terminology and audit repetitive field identities

Merged master !5125 receives extensive Pascal Quantin review aligning MBIM labels with standards terminology and capitalization. The same review catches a copied TD-SCDMA field abbreviation that still used the GSM prefix. When an acronym's meaning was unclear, the contributor quoted the specification rather than guessing an expansion.

**Rule:** field names, labels, acronyms, and capitalization should follow authoritative protocol terminology. Large repetitive `hf_` additions need a copy/paste identity audit covering abbreviations and technology prefixes as well as type, width, and offset.

## A zero-initialized handle may name a real process resource

Merged release-3.6 !5133 fixes MaxMind resolver startup after failed child creation. Closing a zero-initialized child stderr descriptor before confirming spawn success could close fd 0, i.e. stdin.

**Rule:** cleanup of constructor/spawn/open outputs must be gated by successful acquisition or by an explicitly safe invalid sentinel. Zero is not a universal “uninitialized” value for native handles.

## Use packet scope for temporary packet-owned parsing allocations

Merged master !5130 replaces `g_strsplit()`, whose result required explicit cleanup, with `wmem_strsplit(pinfo->pool,...)` in HICP.

**Rule:** when a temporary parse result has packet lifetime, prefer packet-scope wmem allocation over general heap allocation plus manual cleanup. The allocator should express the object's natural lifetime.

## Preserve relevant deprecation notes through the compatibility transition

Merged master !5138 restores a release-note item about the display-filter set-separator deprecation because users crossing the release boundary still need to know why previously accepted syntax is becoming invalid. Duplicate !5141 is closed in favor of it.

**Rule:** do not drop a user-facing deprecation note merely because implementation work has advanced to the next stage. Keep migration information visible through the release in which the compatibility change becomes relevant.

## Aggregate UI labels should use stable semantic identity

Merged master !5117 stops labeling Expert Info groups from the first entry's potentially modified summary and uses the registered expert-field name when possible.

**Rule:** when a model groups multiple events, derive the group label from stable semantic identity rather than arbitrary mutable presentation text from one member.

## Preserve a project-owned CMake module path when dependencies mutate global search state

Merged master !5111 introduces `WS_CMAKE_MODULE_PATH` because Qt 6 extends `CMAKE_MODULE_PATH`. Wireshark-owned modules are then referenced through the stable local path.

**Rule:** if dependency discovery can mutate a global build-system search path, retain a project-owned path for references that must resolve to the project's own modules.

## Lower-weight and superseded evidence

!5145 is useful transitional Qt 6 migration evidence: Gerald Combs and Tomasz Moń's platform testing shows that successful configuration does not prove compilation across macOS and Visual Studio. Later toolchain-baseline work remains more authoritative.

!5140 corroborates reassembly identity/context rules by carrying advertiser address state into chained Bluetooth LE advertising fragments and checking that reassembly produced a tvbuff before subdissection. !5120 shows a conservative temporary response to false-positive fixed-port registration: disabling the default is preferable to systematically stealing ordinary HTTP traffic.

Merged !5116 is intentionally not treated as current precedent for spelling-driven registered-field renames. Later accepted notebook evidence treats display-filter and expert-field abbreviations as user-facing compatibility surfaces. Closed !5141 and !5132 likewise contribute no accepted implementation rule.
