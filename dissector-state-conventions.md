# Wireshark Dissector State Conventions

This file records durable conventions for state maintained by dissectors across packets and dissection passes. Current upstream source remains authoritative.

## Never make dissector correctness depend on packet visitation order

Wireshark does not guarantee that dissectors are called once per packet or in capture-file order. Packets may be dissected multiple times and users can randomly access packets, so mutable static variables that infer protocol state from the sequence of dissector calls are not a valid model for per-flow or per-packet state.

Closed MR !22309 attempted to use static state while adding SMB request/response navigation. Jaap Keuter rejected the design explicitly: "Can't use statics in a dissector. The random access nature of packet access makes temporal order unpredictable." The MR was not merged, so its implementation is negative evidence rather than an accepted patch, but the review statement is a direct architectural constraint from an experienced maintainer and agrees with Wireshark's established conversation, file-scope, reassembly, and first-pass state mechanisms.

**Architecture rule:** if later dissection depends on information learned from another packet, store that information in state keyed by the protocol's real identity and with an appropriate Wireshark-managed lifetime. Do not use a mutable static counter/current-object/previous-packet variable whose meaning depends on dissector invocation order. Where state should only be populated on the first sequential analysis pass, guard updates appropriately and make redissection read the stored result rather than replaying order-dependent side effects.

**Confidence:** High for the architectural rule despite the source MR being closed: the rejection is explicit maintainer feedback and is consistent with accepted state-management patterns elsewhere in the notebook.

## Use protocol-scoped data for dissector-owned transient packet state

Do not treat `pinfo->private_table` as storage privately owned by an individual dissector. It is shared packet state and may be used by other consumers, including Lua. Destroying or repurposing it from one dissector can therefore interfere with unrelated code, and hand-managed containers also create cleanup hazards when dissection exits through an exception.

Merged MR !22163 replaces MKA's `pinfo->private_table` hash with `p_add_proto_data()` / `p_get_proto_data()` using `pinfo->pool` and the MKA protocol ID. The MR explicitly notes both reasons: `private_table` can be used by other dissectors, and scoped proto data removes the need to clean up a manually allocated hash table on exception paths. John Thacker co-authored the change and merged it.

**Architecture rule:** when transient packet state belongs to one protocol, store it with the protocol-data APIs and an appropriate Wireshark-managed allocator/lifetime. Do not appropriate a shared `packet_info` scratch facility as though it were exclusively yours, and prefer scoped ownership that remains correct when exceptions abort dissection.

**Confidence:** Very high. Merged accepted fix with the ownership and exception-safety rationale stated directly.

## Restore temporary per-dissector depth/state before returning

Depth counters and similar `packet_info` state should describe the current nesting stack, not the cumulative number of times a dissector happened to run while dissecting a larger packet. If a dissector increments or otherwise changes temporary nesting state on entry, it must restore that state when its nested call completes.

Merged MR !22158 fixes VLAN depth tracking by restoring the prior VLAN protocol depth after the nested ethertype dissector returns. Without restoration, multiple independent Ethernet frames inside a larger frame were incorrectly treated as ever-deeper VLAN nesting, which forced TECMP to carry a protocol-specific workaround that reset VLAN state itself. The accepted fix removes that cross-dissector workaround because VLAN now owns its own depth lifecycle.

**Architecture rule:** temporary nesting/depth state is stack-like. The dissector that changes it owns restoring it at the matching exit point; sibling or container dissectors should not need protocol-specific resets to compensate for leaked state.

**Confidence:** Very high. Merged master fix authored by John Thacker and approved/merged by Jaap Keuter.

## Match conversation state lifetime to the protocol's logical session, not just its endpoint tuple

A transport endpoint tuple can be reused for multiple independent protocol transactions. Conversation lookup by addresses and ports alone is therefore unsafe when the attached state object models exactly one transfer or session; reusing that conversation can make a later transaction inherit stale state, especially on redissection.

Merged MR !21803, authored by John Thacker and carried to release-4.6, fixes TFTP handling by creating a fresh conversation for every RRQ/WRQ, which is the protocol-defined beginning of a transfer. The MR explains that `tftp_conv_info_t` represents one transfer and cannot simply be reused when ports are reused later in the capture. It also notes the alternative architecture: retain a broader endpoint conversation only if it owns a keyed collection of per-transfer state objects.

**Architecture rule:** identify the lifetime and identity of the state object, not merely the broadest conversation that can find the packet. If one endpoint tuple can carry successive logical sessions, create new state at the protocol's session-start event or key multiple session records beneath the broader conversation. Never let port/address reuse implicitly extend single-session state into a later transaction.

**Confidence:** Very high. Merged John Thacker correctness fix with the state-lifetime mismatch described explicitly and accepted on a stable branch.