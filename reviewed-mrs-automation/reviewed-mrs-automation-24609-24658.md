# Wireshark MR Review Automation Ledger — !24609–!24658

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed MR set from `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, and all per-run files under `reviewed-mrs-automation/`, counting individual MR entries rather than assuming that a numeric range was completely reviewed. The historical !17571–!17620 batch remains explicitly preserved and counted. The fifty highest-numbered corpus MRs not in that set were !24658 through !24609 inclusive.

| MR | Review depth | Outcome / durable evidence |
|---|---|---|
| !24658 | Scanned | Merged Jaap Keuter RTP-MIDI one-byte offset correction; value and quarter-frame type occupy the same octet. Straightforward field-range fix, no new general convention. |
| !24657 | Deep | Merged John Thacker Coverity cleanup adds `ws_assert()` for a call-chain invariant guaranteed by successful `cf_open()`. Useful example of encoding a proven internal invariant for static analysis; no separate rule promoted. |
| !24656 | Scanned | Merged John Thacker ProfileModel NULL-dereference fix scopes `refItem` dereferences to the successful lookup path. Reinforces direct nullability checks. |
| !24655 | Deep | Merged John Thacker SIP cleanup replaces temporary string allocation plus integer conversion with `tvb_get_string_uint()`, uses real `bool` state, and diagnoses invalid values. Reinforces canonical parser/helper usage. |
| !24654 | Scanned | Merged SIP RFC 3329 `mod` parameter support. Protocol-extension maintenance with no broader review lesson. |
| !24653 | Scanned | Merged CI cleanup stops building historical Logray. Build-matrix maintenance only. |
| !24652 | Deep | Merged Michael Mann refactor creates common CLI profile-handling code for tshark/Stratoshark rather than continuing frontend duplication. Useful shared-layer example, but discussion was exploratory rather than a strong new architecture mandate. |
| !24651 | Scanned | Merged ITS generated ASN.1-description update. Generated-protocol maintenance; no new convention. |
| !24650 | Deep | Merged John Thacker SIP fix rewrites delimiter-loop control around `tvb_find_uint8_length()` status/out-parameter semantics rather than preserving an obsolete `-1` sentinel dependency. Promoted to `parser-control-flow-conventions.md`. |
| !24649 | Scanned | Merged capture interface-identification expansion. Platform/interface-name knowledge update, no durable cross-cutting rule. |
| !24648 | Scanned | Merged User Guide documentation for the IPv6 subnets configuration file. Documentation-only. |
| !24647 | Scanned | Merged MS Procmon array-bounds fix. Reinforces validating an index/count against the actual array extent before access. |
| !24646 | Deep | Merged John Thacker wmem API hardening adds `WS_WARN_UNUSED` to allocation/reallocation results. Promoted to `allocator-scope-conventions.md`. |
| !24645 | Scanned | Merged MSYS2 filesystem-location correction. Platform-specific path/install semantics, no broader rule extracted. |
| !24644 | Scanned | Merged Lua Debugger Qt context-menu/gutter/shortcut polish. UI-only. |
| !24643 | Scanned | Merged Lua Debugger layout stabilization. UI-only. |
| !24642 | Scanned | Merged Matter BLE network-recovery advertisement support. Protocol feature addition without reusable review evidence. |
| !24641 | Deep | Merged John Thacker core reassembly bound fix prevents overlap comparison from reading beyond the allocated defragmented buffer. Strong corroboration of existing reassembly-buffer invariants; not duplicated. |
| !24640 | Scanned | Merged macOS setup copy of a libssh warning patch. Dependency/build-maintenance workaround. |
| !24639 | Scanned | Merged macOS setup dependency/version updates and fixes. Build-tool maintenance without a new durable convention. |
| !24638 | Scanned | Merged DHCP option 82/suboption 10 flag dissection. Protocol feature addition. |
| !24637 | Scanned | Merged CMake default install-prefix handling for MSYS2. Platform-specific build configuration. |
| !24636 | Scanned | Merged CMake change avoids discovering Chocolatey tools inside MSYS2 where repository-native packages are expected. Reinforces environment-specific dependency discovery; no general rule promoted. |
| !24635 | Scanned | Merged Lua Debugger header controls. UI-only. |
| !24634 | Scanned | Merged Lua 5.5 test adaptation avoids relying on length behavior of `luaL_[un]ref`'s reusable-reference representation. Compatibility/test maintenance. |
| !24633 | Scanned | Merged Lua 5.5 `lua_newstate` signature compatibility conditional. External-API version adaptation; no new cross-cutting rule. |
| !24632 | Scanned | Merged Lua Debugger theme coloring improvement. UI-only. |
| !24631 | Scanned | Merged synchronization of Wireshark's FindLua with upstream CMake before reapplying local patches. Good vendor/upstream-maintenance practice, but no additional rule extracted. |
| !24630 | Scanned | Merged TPNCP NULL-dereference prevention around parsed data IDs. Reinforces validating parser lookup/state before dereference. |
| !24629 | Deep | Merged packet-frame recolorization fix clears stale `proto_data` before conditionally installing new matches and removes a redundant validity flag that could disagree with the nullable object. Promoted to `state-refresh-conventions.md`. |
| !24628 | Scanned | Merged Qt 6.11 audio deprecation adaptation. Dependency-version compatibility only. |
| !24627 | Scanned | Merged androiddump shadowed-variable cleanup for stricter builds. Compiler-warning portability maintenance. |
| !24626 | Scanned | Merged workaround for the Lua 5.4.5 `lua_resetthread()` API break seen in downstream distributions. Compatibility workaround; no general rule beyond existing external-API boundary guidance. |
| !24625 | Scanned | Merged stable-branch MSYS2 packaging backport accepting whichever supported Lua DLL is present. Backport corroboration only. |
| !24624 | Scanned | Merged stable-branch counterpart of the MSYS2 Lua DLL packaging change. No additional lesson. |
| !24623 | Scanned | Merged GitHub Actions version updates to move actions to Node.js 24. CI dependency maintenance. |
| !24622 | Scanned | Merged GitLab CI retry/timeout tuning for macOS runner instability. Infrastructure-specific mitigation, not a general engineering rule. |
| !24621 | Scanned | Merged default Cisco TTAG decoding for assigned ethertype 0x8905 with standards reference. Protocol registration update. |
| !24620 | Scanned | Merged master MSYS2 packaging change installs the Lua DLL version actually provided by the environment. Packaging compatibility maintenance. |
| !24619 | Deep | Merged DHCP/RADIUS integration calls the existing RADIUS AVP decoder for DHCP option 82/7 and includes a focused capture. Strong corroboration of the existing convention to reuse the canonical dissector/decoder for embedded standard structures. |
| !24618 | Scanned | Merged GitHub Actions cleanup stops installing Perl when the build does not regenerate sources or run Perl-dependent scripts. CI dependency minimization. |
| !24617 | Deep | Merged John Thacker stable-branch hardening disables `actions/checkout` credential persistence in read-only build workflows. Promoted to `ci-security-conventions.md`. |
| !24616 | Deep | Merged John Thacker MSYS2 workflow hardening explicitly propagates PowerShell/process failures and checks expected artifacts/executables. Promoted with !24609 to `ci-security-conventions.md`. |
| !24615 | Scanned | Merged VeriWave stable-branch buffer-length correction. Backport/corroboration of existing buffer-length bookkeeping guidance. |
| !24614 | Scanned | Merged pcapng format-specifier correction for a `sizeof` result. Type/format portability fix; no new rule needed. |
| !24613 | Deep | Merged John Thacker pcapng Darwin PIB UAF fix takes an additional block reference when a second owner (`wth->dpibs`) retains the object, preventing double-unref on error cleanup. Promoted to `allocator-scope-conventions.md`. |
| !24612 | Scanned | Merged stable MSYS2 move to repository-provided Lua 5.4. Packaging/dependency maintenance. |
| !24611 | Scanned | Merged VeriWave master fix keeps buffer length accurate across additional header versions. Reinforces existing buffer metadata invariants. |
| !24610 | Scanned | Merged stable-branch GitHub Actions Node.js-version refresh. Backport/CI dependency maintenance. |
| !24609 | Deep | Merged John Thacker master CI fix prints captured TShark version output before propagating a failing exit code, making an opaque MSYS2 failure diagnosable. Promoted with !24616 to `ci-security-conventions.md`. |

## Promoted durable conventions

- !24650: rewrite parser loop/control flow around the current helper's success/out-parameter contract instead of emulating an obsolete sentinel-return convention.
- !24646: allocation and especially reallocation APIs should be marked must-use when ignoring the returned pointer is almost certainly an ownership/correctness bug.
- !24613: when a reference-counted object gains a second independently releasing owner, acquire a reference for that owner when it is inserted/retained.
- !24629: refreshing derived state must clear the prior value even when recomputation produces no replacement; avoid redundant validity flags that can diverge from the object they supposedly describe.
- !24617: disable persistent checkout credentials in CI jobs that do not need authenticated Git operations after checkout.
- !24616 and !24609: CI wrappers must both propagate child-command failure and preserve the diagnostic output needed to understand that failure.

Other strong cases—including !24641's reassembly bound and !24619's reuse of the canonical RADIUS decoder—corroborate conventions already present in the notebook and were intentionally not duplicated.
