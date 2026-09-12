# Wireshark Dissector State Conventions

This file records durable conventions for state maintained by dissectors across packets and dissection passes. Current upstream source remains authoritative.

## Never make dissector correctness depend on packet visitation order

Wireshark does not guarantee that dissectors are called once per packet or in capture-file order. Packets may be dissected multiple times and users can randomly access packets, so mutable static variables that infer protocol state from the sequence of dissector calls are not a valid model for per-flow or per-packet state.

Closed MR !22309 attempted to use static state while adding SMB request/response navigation. Jaap Keuter rejected the design explicitly: "Can't use statics in a dissector. The random access nature of packet access makes temporal order unpredictable." The MR was not merged, so its implementation is negative evidence rather than an accepted patch, but the review statement is a direct architectural constraint from an experienced maintainer and agrees with Wireshark's established conversation, file-scope, reassembly, and first-pass state mechanisms.

**Architecture rule:** if later dissection depends on information learned from another packet, store that information in state keyed by the protocol's real identity and with an appropriate Wireshark-managed lifetime. Do not use a mutable static counter/current-object/previous-packet variable whose meaning depends on dissector invocation order. Where state should only be populated on the first sequential analysis pass, guard updates appropriately and make redissection read the stored result rather than replaying order-dependent side effects.

**Confidence:** High for the architectural rule despite the source MR being closed: the rejection is explicit maintainer feedback and is consistent with accepted state-management patterns elsewhere in the notebook.