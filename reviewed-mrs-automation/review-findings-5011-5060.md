# Review findings: 5011-5060

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054. All 50 MRs are merged.

- 5060: Guy Harris stable backport; Wireshark-owned sources include config.h unconditionally.
- 5059: Guy Harris and Joao Valverde distinguish external plugin examples from in-tree code; external samples must not assume Wireshark's generated config.h.
- 5058: Master origin of the in-tree unconditional config.h rule.
- 5057: HTTPS setup URL cleanup; Gerald Combs explicitly scoped validation to curl checks, not full script execution.
- 5056: MSYS2 finds system SpeexDSP when not using Wireshark's binary repository.
- 5055: Guy Harris clarifies register_dissector() is general named inter-dissector discovery, not Lua-specific.
- 5054: Release-note organization only.
- 5053: Qt teardown ordering: destroy widgets that require live epan state before epan_cleanup().
- 5052: Release bookkeeping only.
- 5051: Move global public headers under include/ so build/install packaging follows one structural source of truth.
- 5050: Release build bookkeeping only.
- 5049: EBHSCR protocol-specific expansion; no broad convention extracted.
- 5048: PCRE2 migration shows required dependencies must be integrated across CMake, Windows bundles/installers, setup scripts, containers, macOS builders, and runtime packaging.
- 5047: ftypes internal consistency cleanup.
- 5046: 3.2 backport of IEEE 11073 formatter missing-return fix.
- 5045: 3.4 backport of same missing-return fix.
- 5044: 3.6 backport of same missing-return fix.
- 5043: 3.2 backport: pass the actual local buffer capacity, not an unrelated outer-buffer size.
- 5042: 3.4 backport of same buffer-capacity fix.
- 5041: 3.6 backport of same buffer-capacity fix.
- 5040: MinGW/MSYS2 build documentation.
- 5039: MKA backport validates standard-defined body lengths.
- 5038: Same MKA validation backport.
- 5037: Tooling quoting/escaping fix in gen-bugnote.
- 5036: Synthetic malformed-message tree item uses length zero because it has no backing packet bytes.
- 5035: ftypes allocation optimization, consistent with the broader allocator-return API change.
- 5034: ftypes-int.h is private to ftypes; outside callers move to public functions/accessors.
- 5033: RTPS field-abbreviation correction; later compatibility guidance means this is not a precedent for casual renames.
- 5032: Lua capitalization cleanup only.
- 5031: CDR representation version can change alignment independently of scalar width.
- 5030: Secondary display-filter editors keep diagnostics local instead of overwriting the main-window status bar.
- 5029: Stable revert of an earlier Qt cleanup fix; negative/supersession evidence only.
- 5028: Lua/funnel dialogs become children of the main window so Qt ownership drives predictable destruction.
- 5027: Master revert of the earlier Qt cleanup fix; negative/supersession evidence only.
- 5026: Generated ASTERIX source records upstream specification revision.
- 5025: GVSP protocol-specific field additions.
- 5024: Stable MKA body-length validation backport.
- 5023: 3.2 TCP ownership backport: return a caller-freeable allocation on every path.
- 5022: 3.4 backport of same TCP ownership fix.
- 5021: 3.6 backport of same TCP ownership fix.
- 5020: John Thacker converts DLEP TCP handling to tcp_dissect_pdus(), supporting split and coalesced PDUs.
- 5019: Stable AVTP/IEEE1722 backport passes a subset tvbuff after the UDP sequence-number shim.
- 5018: Master TCP fix: do not return a static literal from an API whose result callers may free.
- 5017: Guy Harris explains that wmem-allocating formatters remove the need for a separate representation-length API; the rationale belongs in commit/MR text.
- 5016: Display-filter error handling centralizes coupled error-recording and exception behavior.
- 5015: Display-filter semantic checks carry the function name and produce constraint-specific diagnostics.
- 5014: Display-filter semantic-check cleanup; mostly refactor.
- 5013: Master AVTP/IEEE1722 fix roots downstream parsing at the true payload with a subset tvbuff.
- 5012: Stable tap-cleanup backport resets freed globals to NULL.
- 5011: Master tap cleanup restores freed global lists to NULL so later destructors cannot traverse stale pointers.

Strongest themes: extension/private-header boundaries (5059, 5034); dependency integration completeness (5048); uniform ownership contracts (5018); TCP framing and child-tvb boundaries (5020, 5013); and teardown ordering plus neutral post-cleanup state (5053, 5028, 5011).
