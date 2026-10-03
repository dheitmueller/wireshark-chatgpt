# Wireshark Conversation State Scope Conventions

This file records durable conventions for choosing the ownership, lifetime, and lookup key of protocol state that persists across packets. Current upstream architecture and protocol semantics remain authoritative.

## Store negotiated state at the protocol scope that owns it

Persistent dissector state should live at the semantic scope at which the protocol defines or negotiates it. A flag that applies to an entire transport connection must not be stored independently in a narrower child context merely because that child context is where the flag is first observed; doing so can cause parallel child contexts on the same transport to disagree about state that is actually shared.

Merged MR !15491 fixes DCE/RPC header-signing state. `PFC_HDR_SIGNING` is negotiated once for a transport connection and then applies to all authentication contexts on that connection. The accepted implementation therefore introduces a transport-level `dcerpc_connection` object rather than retaining the property in authentication-context state. Its lookup key combines the Wireshark conversation with a transport salt so that the persistent object represents the actual connection identity rather than just one authentication exchange. John Thacker reviewed the key/hash implementation and merged the change.

**Implementation rule:** derive persistent state ownership from the protocol's semantic lifetime: packet, transaction, stream, authentication context, transport connection, or another explicitly defined scope. Narrower objects may reference state owned by a wider scope, but should not maintain independent copies of a property whose protocol lifetime is wider.

**Keying rule:** a state object's lookup key must contain every identity component needed to distinguish the semantic owner across multiplexing, reconnection, or transport reuse. A convenient existing conversation pointer is not sufficient by itself if the protocol or transport layer requires an additional discriminator.

**Review rule:** for each new persistent flag or negotiated parameter, ask both “who negotiates this?” and “for how long does it apply?” Then verify that the state container and key match those answers rather than the location in the code where the value happens to be parsed.

**Confidence:** Very high. Merged master correctness change with a protocol-specific ownership rationale, substantive John Thacker review, and John Thacker merge.


## Do not use process-global variables for state that can differ by capture or flow

A dissector-global mutable variable is not reset simply because the user opens a new capture, and it cannot distinguish simultaneous flows that legitimately carry different protocol state. Persistent values that vary by conversation therefore need a conversation/context owner; values repeated in every packet may be kept local instead of being persisted at all.

During review of merged master MR !505, Pascal Quantin explicitly rejected global RTPS parameters for both reasons: they would survive capture-file changes and would break when different flows were mixed in one capture. The contributor removed the globals and used packet-local values because the needed version/vendor information was present in each packet.

The same review also requested resolving reusable subdissector handles in `proto_reg_handoff_*` rather than repeatedly looking them up for each packet, and checking the cached handle before dispatch.

**State rule:** before adding mutable file-scope state, ask whether it is truly process-global configuration. If it describes a flow, transaction, or capture-derived protocol condition, store it at that semantic scope; if each packet carries the value, prefer local decode state.

**Dispatch rule:** reusable dissector handles are registration/handoff state, not packet state. Resolve them once during handoff and validate the handle before calling it.

**Confidence:** Very high. Direct Pascal Quantin review was incorporated into a merged master dissector.
