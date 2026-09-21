# Wireshark Protocol Enablement and Dispatch Conventions

This file records durable conventions for dispatch paths that invoke protocol-specific dissectors or decode helpers outside ordinary dissector-table registration. Current upstream implementation remains authoritative.

## Indirect protocol-specific callbacks must honor protocol enablement

Generic container dissectors sometimes maintain their own callback registries for protocol-specific interpretation. Because those registries sit outside the normal dissector-table dispatch machinery, they can accidentally continue invoking a disabled protocol even after the user has turned that protocol off.

Merged master MR !14609, authored and merged by John Thacker, fixes this in the JSON dissector. JSON keeps a hash from member-name fields to protocol-specific decoding functions; the generic path could therefore invoke JSON-3GPP for a matching key such as `supportedFeatures` even when the JSON-3GPP protocol was disabled. The accepted fix finds the protocol owning the registered field and checks `proto_is_protocol_enabled()` before returning the callback. The accompanying code comment also notes that a normal dissector-table mechanism would be preferable because it would carry standard protocol-dispatch behavior rather than reproducing it manually.

**Implementation rule:** any custom registry or callback path that crosses from a generic dissector into protocol-specific decoding must preserve the same enable/disable semantics users get from ordinary dissector dispatch. Determine the target protocol owner and decline the callback when that protocol is disabled.

**Architecture rule:** prefer standard dissector-table or comparable framework dispatch when it naturally represents the lookup. Framework-owned dispatch reduces the chance that custom registries omit enablement, Decode-As, preference, lifecycle, or other protocol-level semantics.

**Review rule:** when adding an indirect callback registry, ask which behaviors would normally be supplied by a dissector table and verify that the custom path intentionally preserves each relevant one. A successful key lookup is not, by itself, permission to invoke a disabled protocol.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker, with the missing protocol-enable check and the architectural preference for normal dissector-table dispatch both explicit in the accepted change.
