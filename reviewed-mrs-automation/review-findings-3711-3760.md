# Review findings: Wireshark MRs !3711–!3760

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed the 50 highest-numbered corpus MRs below the prior frontier after reconciling the available review tracking. Merged work is weighted more heavily than closed/superseded work; direct maintainer guidance is noted where it provides durable review evidence.

| MR | State | Finding |
|---|---|---|
| !3760 | merged | HTTP/3 SETTINGS dissection; useful protocol coverage but no durable review discussion beyond the accepted implementation. |
| !3759 | merged | Adds retry behavior to Windows CI jobs; operational CI resilience change. |
| !3758 | merged | Martin Mathieson extends `check_typed_item_calls.py` to the `proto_tree_add_bitmask*` family, modeling its different hf argument position and allowed field types. |
| !3757 | merged | Guy Harris changes a BTATT aggregate field from `FT_NONE` to `FT_UINT24`; bitmask/container fields must have a numeric type compatible with the API that consumes them. |
| !3756 | merged | Same Guy-authored BTATT field-contract repair in another characteristic. |
| !3755 | merged | Same Guy-authored BTATT repair; Martin Mathieson explicitly points to !3758 as the checker coverage that will catch this class of error automatically. |
| !3754 | merged | Adds F1AP statistics UI. Pascal Quantin questions redundant Info-column append behavior already provided by other 3GPP dissectors. |
| !3753 | merged | Guy Harris code organization cleanup in the 3GPP 32.423 Wiretap reader. |
| !3752 | merged | Large Thrift Binary/Compact completion: reassembly-aware helper APIs, exported ABI updates, subdissector option context, fuzzing and real-life Jaeger validation; Anders Broman requests squash before merge. |
| !3751 | merged | wslog cleanup; no durable discussion. |
| !3750 | merged | Fixes an uninitialized Wiretap variable. |
| !3749 | merged | Initial macOS ARM CI. Guy Harris argues for separate macOS x86 pre-merge coverage so generic macOS failures can be distinguished from ARM-specific failures. |
| !3748 | merged | Automatic registry/manufacturer update. |
| !3747 | merged | Automatic registry/manufacturer update. |
| !3746 | merged | Automatic registry/manufacturer/translation update. |
| !3745 | merged | Martin Mathieson adds a checker warning for masks spelled as multi-digit all-zero hex values and normalizes existing registrations to `0x0`; metadata/readability hygiene rather than protocol semantics. |
| !3744 | merged | Documentation typo fix. |
| !3743 | merged | Removes unused CMake definitions. |
| !3742 | merged | Evan Huus converts ASN.1 dissectors to `pinfo->pool`, including authoritative ASN.1 templates/configuration and regenerated output. |
| !3741 | merged | WOWW decryption refactor supports multiple messages per PDU and stops when server data cannot be decrypted. |
| !3740 | closed | Pascal Quantin says the MR is too large for GitLab review, requires splitting, and rejects reintroducing deprecated `wmem_packet_scope()` that earlier work intentionally replaced with `pinfo->pool`. |
| !3739 | merged | CMS ASN.1 correction with regenerated dissector output. |
| !3738 | merged | Guy Harris adds IPv6 support to rpcap findalldevs dissection. |
| !3737 | merged | CIP Motion terminology update to match specification. |
| !3736 | merged | Alexis La Goutte catches an `FT_UINT8` field read with length 2 and identifies `tools/check_typed_item_calls.py` as the checker/CI mechanism. |
| !3735 | merged | Restricts LTO to explicit release-oriented configuration; discussion favors opt-in behavior over surprising optimization in the default RelWithDebInfo workflow. |
| !3734 | merged | Profile ZIP import raises per-file cap from 512 KiB to 256 MiB. Roland Knall explains the cap was a runaway guard for malformed ZIPs; Guy Harris asks for the actual infinite-loop failure mode. |
| !3733 | merged | DoIP validates payload length more carefully. |
| !3732 | merged | IEEE 802.11 ranging NDP handling fix. |
| !3731 | closed | Diameter dictionary submission superseded after Anders Broman notes unrelated changes and uncertainty around type conversion; successor work was separated. |
| !3730 | merged | MP4 handles missing timescale. |
| !3729 | merged | New HICP dissector; review asks for a sample capture and proper rebase before merge. |
| !3728 | merged | PFCP update to a newer 3GPP specification version. |
| !3727 | merged | CMake fix for macOS systems with both Qt5 and Qt6 installed. |
| !3726 | merged | eCPRI aggregate header subtree no longer displays an artificial UINT32 value. |
| !3725 | merged | MKA version 3 implementation rather than pretending unsupported content is dissected. |
| !3724 | merged | Broad `pinfo->pool` conversion. João Valverde warns that generated ASN.1 dissectors must be changed at their ASN.1 source/template and regenerated after automated conversion; !3742 follows through. |
| !3723 | merged | RADIUS H3C dictionary update. |
| !3722 | merged | eCPRI/O-RAN comments and long descriptions. |
| !3721 | merged | 3GPP JSON correction plus GTPv2 crash guard; Anders Broman questions whether packet-info state should ever be absent, highlighting the need to understand the invalid state rather than only guard it. |
| !3720 | merged | GTPv2 EN-DC SON Configuration IE dissection. |
| !3719 | merged | Guy Harris ensures text import creates a `WTAP_BLOCK_PACKET` before adding packet options and releases it after write. |
| !3718 | merged | Guy Harris adds packet-flags metadata only when direction is actually present; optional metadata presence must not be synthesized from a default value. |
| !3717 | merged | Removes unnecessary GLib libraries from CMake target link lists. |
| !3716 | merged | Adds Gcrypt specifically to `sdjournal_LIBS`; accepted focused fix after closed !3713 proposed making Gcrypt PUBLIC for all wsutil consumers. |
| !3715 | merged | New FiveCo Legacy dissector; review favors existing value-string helpers, removing template leftovers, avoiding unnecessary `if (tree)`, and using the normal offset idiom. |
| !3714 | closed | Documentation-warning fix closed because the changes already existed on master. |
| !3713 | closed | Proposed making Gcrypt PUBLIC on wsutil to fix sdjournal linkage. Gerald Combs instead asks to link Gcrypt only into sdjournal because most wsutil consumers do not require it; accepted !3716 implements that narrower dependency edge. |
| !3712 | merged | New SHICP dissector. Review requires license declaration and sample capture; Jaap Keuter challenges a UDP heuristic when the protocol has a fixed port, preferring direct `udp.port` registration as more efficient. |
| !3711 | merged | WiMAX display-filter abbreviation fix; no durable discussion. |

## Durable conventions extracted

1. **Static checker coverage should follow API semantics.** !3758 extends the checker to every `proto_tree_add_bitmask*` variant and handles the hf argument position; !3755–!3757 show the concrete field-contract bug this catches.
2. **Generated dissector migrations must update the source of truth.** !3724's broad conversion initially touched generated ASN.1 C; João Valverde explicitly requires changing the ASN.1 inputs and regenerating. !3742 is the accepted follow-through.
3. **Packet-local allocations should use explicit packet context.** Merged !3724/!3742 continue the project migration to `pinfo->pool`; closed !3740 is negative evidence that a large submission should not reintroduce `wmem_packet_scope()`.
4. **CI matrices should separate platform family from architecture-specific failures.** In !3749, Guy Harris argues for macOS x86 pre-merge coverage alongside macOS ARM.
5. **Safety limits need a stated failure model and malformed-input tests.** !3734 accepts a much larger profile-file cap because real configurations exceed the old arbitrary limit, while review focuses on the malformed ZIP/runaway behavior the guard was intended to contain.
6. **Optional capture metadata requires an existence contract.** !3718 records packet direction only when direction is known, while !3719 constructs the correct packet block before attaching options.
7. **Link dependencies at the narrowest target that requires them.** Closed !3713 proposed making Gcrypt PUBLIC through wsutil; Gerald Combs instead recommends adding Gcrypt specifically to sdjournal, and merged !3716 is that focused fix.
8. **Prefer deterministic table dispatch over broad heuristics when the protocol has a reliable fixed binding.** In merged !3712, Jaap Keuter explicitly prefers direct fixed-port registration.
9. **Submission size is a reviewability constraint.** In closed !3740, Pascal Quantin requires an MR too large for GitLab's review UI to be split into smaller reviewable units.
