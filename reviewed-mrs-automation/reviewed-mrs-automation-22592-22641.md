# Wireshark MR review batch: !22641–!22592

Corpus commit used: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Exactly 50 previously unreviewed corpus MRs were reviewed newest-to-oldest. The already-reviewed set was reconstructed from `reviewed-mrs.md` and the available per-run files under `reviewed-mrs-automation/`; individual MR entries, not inferred numeric ranges, were used for de-duplication. The historical !17571–!17620 batch remains preserved and counted.

Exact reviewed MR set, in review order:

!22641, !22640, !22639, !22638, !22637, !22636, !22635, !22634, !22633, !22632, !22631, !22630, !22629, !22628, !22627, !22626, !22625, !22624, !22623, !22622, !22621, !22620, !22619, !22618, !22617, !22616, !22615, !22614, !22613, !22612, !22611, !22610, !22609, !22608, !22607, !22606, !22605, !22604, !22603, !22602, !22601, !22600, !22599, !22598, !22597, !22596, !22595, !22594, !22593, !22592.

## Review notes

| MR | Weight | Review result |
|---|---|---|
| !22641 | Deep / merged | Michael Mann exports APIs used by dissectors with `WS_DLL_PUBLIC`, making plugin use possible. Strong corroboration of existing public/plugin API guidance; no duplicate rule added. |
| !22640 | Deep / merged; promoted | John Thacker fixes Thrift varint decoding so width-specific maximum encoded lengths are actually enforced; resolves OSS-Fuzz. Promoted to `input-resource-limit-conventions.md`. |
| !22639 | Scanned / merged | Re-adds BLF uncompressed-segment length validation after the temporary revert in !22634. Useful accepted end state; format-specific. |
| !22638 | Scanned / merged | Stable release version bump. No engineering convention. |
| !22637 | Scanned / merged | Stratoshark inherits project patch/build/version extension and bumps its version. Corroborates product/application version-policy separation. |
| !22636 | Deep / merged; promoted | Michael Mann moves knowledge of protocol registration sets to the application layer and has generic epan/proto initialization invoke supplied callbacks. Promoted to `application-layer-boundary-conventions.md`. |
| !22635 | Scanned / merged | Stable release build/version housekeeping. |
| !22634 | Scanned / merged transitional | Temporarily reverts BLF uncompressed-segment validation, later restored by !22639. Down-weighted as a temporary state. |
| !22633 | Deep / merged | Lua `DissectorTable_try()` stops automatically invoking the data dissector so it can return 0 as documented. Reinforces API behavior matching documented return semantics. |
| !22632 | Corroboration / merged | HomePlug AV column update fix related to !22630; removes dependence on tree presence for column behavior. |
| !22631 | Corroboration / merged | HomePlug AV column update fix related to !22630; same semantic lesson. |
| !22630 | Deep / merged | John Thacker removes a NULL-tree condition that incorrectly suppressed `COL_INFO` updates when a real/custom-column tree exists. Reinforces that tree presence is not a proxy for whether non-tree semantic side effects are needed. |
| !22629 | Low / open draft | Supporters-tab UI draft with a failed pipeline and no substantive review in the snapshot. Deliberately down-weighted. |
| !22628 | Corroboration / merged | Refactors post-decrementing DCOM loops that leave an unsigned condition variable wrapped on the failing test. Reinforces arithmetic/static-analysis hygiene. |
| !22627 | Scanned / merged | Corrects NMEA0183 field representation from bytes to string. Field-semantic correction without a new general rule. |
| !22626 | Scanned / merged | Adds basic NMEA2000 dissection. Feature addition. |
| !22625 | Scanned | Qt keyboard-shortcut UI relocation/printing feature. No durable core convention promoted. |
| !22624 | Deep / merged release | Restores `ws_base32_decode()` after ABI breakage; John Thacker approved. Strong corroboration of existing stable-branch ABI preservation guidance. |
| !22623 | Scanned / merged | Makes `asn2wrs.py` relative-path handling work across Windows drive letters. Portability-specific tooling fix. |
| !22622 | Scanned / merged | Small QUIC correctness cleanups; no distinct durable lesson. |
| !22621 | Scanned / merged | Adds WFA Capabilities subtype dissection. Feature addition. |
| !22620 | Deep / merged | Fixes misuse of a negative offset with `tvb_bytes_exist`; reinforces explicit offset/length-domain and tvbuff bounds guidance. |
| !22619 | Scanned / merged | Adds compressed RDP dynamic-virtual-channel support and hardens related parsing. Protocol-specific hardening. |
| !22618 | Scanned / merged | CI workaround for macOS Intel clock synchronization. Environment-specific. |
| !22617 | Scanned / merged | Acknowledges the jtckdint checked-arithmetic dependency. Licensing/attribution maintenance. |
| !22616 | Corroboration / merged | Converts `tvb_ensure_*_length_remaining` offsets to unsigned where negative offsets are not meaningful. Reinforces API-domain migration guidance. |
| !22615 | Corroboration / merged | Refactors ALP post-decrement loop to avoid wraparound/static-analysis complaints. |
| !22614 | Deep / merged | Uses checked addition for CBOR signed arithmetic instead of testing an already-overflowed result. Reinforces checked-arithmetic guidance. |
| !22613 | Scanned / merged | Replaces fixed `snprintf` assembly with `wmem_strbuf_t` in packet-ip, also avoiding old-compiler warnings. |
| !22612 | Deep / merged documentation | README.developer explicitly recommends `tvb_reported_length_remaining()` over subtraction because subtraction can overflow/underflow. Strong corroboration of existing tvbuff arithmetic rules. |
| !22611 | Corroboration / merged | Retrieves XTP reported length remaining at the point of use to avoid stale/overflow-prone derived lengths. |
| !22610 | Scanned / merged | jtckdint attribution counterpart to !22617. |
| !22609 | Corroboration / merged | Retrieves Wi-Fi DPP reported length remaining when needed instead of decrementing a cached remainder. Reinforces direct remaining-length APIs. |
| !22608 | Deep / merged | Prevents X11 message-step overflow when adding a decoded message length to the current offset. Reinforces checked offset advancement. |
| !22607 | Deep / merged | Reverts removal of `ws_base32_decode()` because the exported symbol is part of the ABI and cannot disappear before a major SO-version boundary. Strongly corroborates `abi-compatibility-conventions.md`. |
| !22606 | Scanned / merged | Adds S7Comm-Plus dissector support. Feature addition. |
| !22605 | Scanned / merged | Adds IEEE 802.11 DMS Request/Response elements. Feature addition. |
| !22604 | Scanned / merged | Suppresses expected libxml parser warnings in nettrace loading. Narrow diagnostics behavior. |
| !22603 | Deep / merged | Validates BLF uncompressed segment length against stored data length; later temporarily reverted/restored. Retained as format-boundary evidence, with !22639 representing the accepted final state. |
| !22602 | Corroboration / merged | Related BLF uncompressed-segment length validation in another branch/context. |
| !22601 | Deep / merged | Enables `-Wpointer-sign` and consequently full `-Wall -Wextra` coverage everywhere after ASN.1 cleanup. Strong build-hygiene evidence; existing warning-enforcement guidance already covers it. |
| !22600 | Discussion-focused / merged | Guy Harris separates ASN.1 whitespace cleanup so !22599's functional diff stays smaller. Strong authoritative evidence for keeping mechanical cleanup separate from functional changes; existing focused-diff/submission guidance already covers it. |
| !22599 | Scanned / merged | Restores MMS ASN.1 REAL dissection now supported by generator/runtime. Feature restoration; benefited from !22600's separate whitespace cleanup. |
| !22598 | Scanned / merged | Parallelizes spelling checker using `ProcessPoolExecutor`; tooling performance improvement. |
| !22597 | Scanned / merged | Stable release version bump. |
| !22596 | Deep / merged | Hides protocol registration/handoff declarations that need not be public headers. Corroborates API-surface minimization and registration ownership. |
| !22595 | Deep / merged release; promoted | John Thacker caps PKCS#12 hash iteration counts to prevent attacker-controlled extreme CPU work and fixes signed-to-unsigned work-factor conversion. Promoted to `input-resource-limit-conventions.md`. |
| !22594 | Deep / merged release; promoted | Same PKCS#12 resource-limit fix on another maintained release branch, independently reinforcing the accepted policy. |
| !22593 | Scanned / merged | Stable release build housekeeping. |
| !22592 | Deep / merged | Extends static checker coverage to detect `expert_add_info_format()` uses where plain `expert_add_info()` suffices. Reinforces encoding review conventions in executable tooling rather than relying only on manual review. |

## Durable notebook changes

- Added `input-resource-limit-conventions.md`: cap attacker-controlled expensive work factors and enforce width-specific variable-length integer representation limits inside the decoder, based on !22594/!22595 and !22640.
- Extended `application-layer-boundary-conventions.md` with !22636: applications own the choice of protocol registration sets; generic epan/proto code supplies the registration mechanism.
- !22624/!22607, !22641, !22630–!22632, !22620/!22612/!22609, !22614/!22608, !22601, !22600, and !22596 were retained as strong corroboration of already-existing notebook rules instead of duplicating them.
- Open draft !22629 and temporary revert !22634 were explicitly down-weighted.