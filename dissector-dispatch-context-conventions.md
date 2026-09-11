# Wireshark Dissector Dispatch Context Conventions

This file records durable conventions for metadata and extension points around nested dissector dispatch. Current upstream source remains authoritative.

## `packet_info` match metadata should identify the selector actually used

When a parent protocol invokes an application dissector through a selector such as ALPN or a port, populate the corresponding `packet_info` match metadata before the child call, even when the selected dissector handle was cached earlier. Child dissectors may legitimately use that metadata to understand why they were selected.

Merged MR !25754, authored and merged by John Thacker, makes TLS and DTLS set `pinfo->match_string` to the negotiated ALPN before dispatching decrypted application data, analogous to the existing use of `pinfo->match_uint` for the application port. NTS-KE consequently reads the matched ALPN from `pinfo->match_string` instead of using a TLS-internal accessor that is not part of libwireshark's public interface.

Because `packet_info` is shared through nested dissector calls, the parent saves the previous `match_string` / `match_uint` values and restores them after the child returns.

**Implementation rule:** dispatch metadata should describe the real registration/selection key presented to the child. Any temporary mutation of shared `packet_info` dispatch state must be save/set/call/restore scoped so nested dissection does not leak its selector into unrelated siblings or parents.

**Confidence:** Very high. Merged master dispatch/API cleanup authored and merged by John Thacker.

## Decode-As dissectors must use the matched selector, not a hard-coded default port

A dissector that can be selected through Decode As must distinguish the port or other selector that actually caused dispatch from the protocol's conventional/IANA default. Direction decisions, nested dispatch context, and save/restore logic based on the default value are wrong when the user deliberately binds the dissector to another port.

Merged MR !23597 fixes SOCKS on the supported release branch by comparing the destination port with `pinfo->match_uint` rather than the fixed SOCKS port when determining direction, and by preserving/restoring the actual matched transport-port context around a nested dissector call. The accepted change was merged by Jaap Keuter and approved by Michael Mann.

**Implementation rule:** when behavior depends on the selector through which a dissector was invoked, use the dispatch metadata (`pinfo->match_uint`, `match_string`, or the corresponding registration context), not the protocol's default constant. Preserve and restore the real caller context around nested calls.

**Confidence:** High. Merged correctness backport with maintainer approval, and it directly reinforces the more general merged-master dispatch-metadata rule above.

## Use dissector tables for protocol-defined vendor extension namespaces

When a protocol deliberately reserves a numeric namespace for manufacturer/vendor-specific payloads, a dissector table is the normal extensibility mechanism. This allows built-in dissectors and external Lua/native plugins to register independently without teaching the core dissector about every vendor.

Merged MR !25741 adds an RDM manufacturer-specific PID dissector table and demonstrates it with both a packet capture and a Lua plugin. The core RDM dissector derives the combined manufacturer/PID key and dispatches through the table, while known built-in manufacturer handling can live behind the same extension boundary.

**Implementation rule:** model protocol-defined extension namespaces as registration points rather than hard-coded private switches. For plugin-facing tables, validate both the wire dispatch and a realistic external registration path.

**Confidence:** High. Merged master extension design with sample capture and Lua-plugin validation.
