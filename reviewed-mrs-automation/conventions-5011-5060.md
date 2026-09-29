# Conventions extracted from MRs 5011-5060

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

## Build and extension boundaries

Wireshark-owned source may require the generated config.h unconditionally, but out-of-tree plugin examples must not assume that Wireshark-private generated header exists. Merged 5058/5060 establish the in-tree side; Guy Harris's merged 5059 explicitly drives the external sample toward no config.h dependency. Later 5063 remains the strongest external-plugin evidence.

A new required dependency is repository-wide integration work. Merged 5048 made PCRE2 required and the review tracked CMake discovery, Windows bundles and installers, setup scripts, containers, macOS builders, and runtime DLL packaging. A successful compile alone does not prove packaged runtime completeness.

Merged 5051 moves global public headers under include/ and lets packaging consume that structure rather than maintain a parallel hand-curated inventory.

## API and module boundaries

Merged 5034 confines ftypes-int.h to the ftypes implementation and moves outside callers to public functions/accessors. Private *-int.h headers should not become de facto cross-subsystem APIs.

Guy Harris's merged 5055 clarifies that register_dissector() provides named discovery for other dissectors; it is not a Lua-specific chaining facility.

In merged 5017, Guy Harris explicitly notes that once formatting routines return wmem-allocated strings, the separate "get required representation length" API becomes unnecessary. Remove redundant paired sizing APIs when allocation ownership moves into the callee, and describe the architectural reason in the commit/MR message.

## Ownership and cleanup

Merged master 5018 fixes tcp_flags_to_str() so every return path honors the same caller-freeable ownership contract; stable 5021-5023 corroborate it. Never return a static literal from a function whose normal result is caller-owned allocated storage.

Merged 5053 documents teardown dependency order: Qt widgets that assume live epan data must be destroyed before epan_cleanup(). Merged 5028 makes Lua/funnel dialogs children of the main window so Qt ownership produces predictable destruction.

Merged master 5011 and stable 5012 reset tap globals to NULL after freeing the lists. Cleanup should restore externally reachable global state to a safe empty sentinel rather than leave dangling pointers for later destructors.

## Framing and tvbuff boundaries

Merged 5020 uses tcp_dissect_pdus() for DLEP so TCP handling supports both one PDU split across segments and multiple PDUs coalesced in one segment.

Merged master 5013 and stable 5019 strip the UDP-specific AVTP sequence-number shim by creating a subset tvbuff rooted at the actual AVTP payload before calling subdissectors. Transport-specific prefixes should not leak into semantic child dissectors.

Merged 5031 also shows that encoding-version alignment rules must be modeled explicitly; alignment is not always the same as scalar byte width.

## Presentation and diagnostics

Merged 5036 uses source length zero for a synthetic malformed-message string with no backing packet bytes. Generated tree values should not claim unrelated packet ranges.

Merged 5030 scopes display-filter feedback to the UI surface that owns the edit. Secondary dialogs should not overwrite a shared main-window status channel when they already have local hint/error presentation.

Stable 5041-5043 fix a bounded-formatting bug by passing sizeof(the actual local buffer), not an outer output-buffer size. Capacity values belong to the destination buffer actually being written.

## Supersession

Merged 5027 and 5029 revert an earlier Qt epan-cleanup fix, so the reverted implementation is negative history rather than accepted architecture. Later accepted lifecycle guidance remains authoritative.

Merged 5033 corrected an RTPS field abbreviation, but later Wireshark review treats registered field names as compatibility surfaces. Do not generalize this historical correction into a rule permitting casual field-abbreviation renames.
