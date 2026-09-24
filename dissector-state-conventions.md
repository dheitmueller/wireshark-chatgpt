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

## Match transient state to a single dissection pass when that is the semantic lifetime

A frame can be dissected repeatedly, and a protocol dissector can also be invoked multiple times while dissecting one frame in a single pass. State whose only purpose is to distinguish the first invocation from later invocations in that same pass should reset naturally before the next pass; carrying it across passes makes redissection history affect presentation.

Merged MR !20414 spent several review iterations on SMB column handling. John Thacker demonstrated that a static `last_fnum` could incorrectly classify the first SMB command when the GUI redissected the same frame after changing colorization. His preferred model was protocol data in `pinfo->pool`: it starts empty on each pass, can record that SMB has already been invoked for the current frame during that pass, and disappears before a later pass. He explicitly preferred `p_add_proto_data()`/`p_get_proto_data()` over globals where possible; `register_frame_end_routine()` was described as a fallback for pre-existing global state rather than a reason to introduce it.

**Architecture rule:** identify whether state is per invocation, per frame/pass, per packet across passes, per conversation, or per file. For same-pass coordination between repeated calls of one dissector, prefer protocol-scoped data with `pinfo->pool` so redissection begins from a clean transient state. Do not infer same-pass history from a static frame number that survives into a later pass.

**Review implication:** exercise GUI-triggered redissection paths when state affects columns or presentation. A test that only walks the capture sequentially once can miss stale-state bugs that appear when the same frame is dissected twice in succession.

**Confidence:** Very high. The MR was eventually merged after a detailed multi-month review, and the pass-lifetime reasoning comes directly from John Thacker's concrete reproducer and proposed state model.

## Treat protocol-data keys as a namespace, not as interchangeable labels

`p_add_proto_data()` / `p_get_proto_data()` identify an entry by the protocol ID and key (plus any layer bits used by the caller). Two logically independent state records stored under the same protocol ID must therefore use distinct keys. Reusing a key because two records have the same C type does not make them the same state object; one path can overwrite or retrieve the other's data and then interpret unrelated or uninitialized state as its own.

Merged MR !26517 fixes EAP/LEAP frame state after LEAP reused EAP's `PROTO_DATA_EAP_FRAME_STATE` key. Both paths stored `frame_state_t`, but they represented different state machines. The collision caused them to stomp on one another and led to uninitialized-memory access. The accepted fix introduces a dedicated `PROTO_DATA_EAP_FRAME_STATE_LEAP` key and was approved and merged by John Thacker.

**Architecture rule:** allocate protocol-data keys by semantic state identity. If multiple subprotocols, modes, or independent analyses use the same `proto_id`, give each independent record a distinct key (while preserving any deliberate layer component). Do not reuse a key merely because the payload struct type is identical.

**Confidence:** Very high. Direct merged correctness fix with the key collision and resulting memory-safety failure stated explicitly.

## Store derived state at the scope that owns its semantic identity

A convenient transport conversation is not necessarily the semantic owner of information derived while dissecting it. If one transport conversation can legitimately carry multiple unrelated higher-level transactions, attaching transaction-specific state to the transport conversation can make the later transaction inherit or overwrite another transaction's state.

Merged master MR !15398, authored by John Thacker and merged by Anders Broman, fixes SDP-generated Call-ID handling. The same SDP transport connection can be reused by unrelated calls, so the accepted code no longer stores the generated Call-ID list on the SDP conversation. It transfers the data transiently through packet protocol data and, when SIP has the appropriate file-scoped offer/answer `transport_info_t`, stores it with that transaction-level object; standalone SDP remains packet-scoped.

**Architecture rule:** choose state storage by the identity and lifetime of the information, not by the broadest conversation object that happens to be available. When a lower-level connection can host several independent higher-level transactions, keep transaction-specific state on the transaction/session object, using packet-scoped handoff data when necessary to move the result between dissector layers.

**Review rule:** for every new conversation proto-data field, ask whether the underlying endpoint tuple can be reused by multiple semantic owners. If it can, either key the state by the higher-level identity or attach it to a narrower state object whose lifetime matches that identity.

**Confidence:** Very high. Merged master correctness fix authored by John Thacker; the MR description explicitly identifies transport-connection reuse as the bug and documents the accepted packet/file-scope split.

## Do not let speculative lookup create a protocol session

A helper that looks like a read/query operation may internally have get-or-create behavior. Calling such a helper speculatively while probing alternative protocol interpretations can therefore mutate conversation state even when that protocol is not actually present, causing later dissectors to observe a session that was invented by the probe itself.

Merged master MR !15023, authored and merged by John Thacker and approved by Pascal Quantin, fixes JSON-3GPP handling for 5GC traffic. The code used `http2_get_header_value()` to look for HTTP/2 headers, but that path ultimately called `get_http2_session()` and created HTTP/2 session state even for OAI traffic carried over HTTP/1.1. The resulting synthetic state could confuse the HTTP dissector. The accepted implementation first verifies that HTTP/2 is actually a protocol in the frame before using the HTTP/2 helper and separately supports the real-world HTTP/1.1 variant. Release-4.2 MR !15040 carries the same fix.

**Architecture rule:** before invoking a helper that may allocate or attach conversation/session state, establish authoritative evidence that the corresponding protocol context exists. Prefer distinct read-only lookup and get-or-create APIs when callers need both behaviors; a query-shaped helper should not silently create state during speculative dispatch.

**Review rule:** audit the side effects of helper calls used in heuristics, fallback parsing, and multi-protocol dispatch. A function name such as `get_*` or `*_header_value` is not proof that the operation is observationally pure.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker, with an accepted stable backport.

## Key negotiated protocol-version state by the conversation that negotiated it

Protocol version is often session state, not process-global state. A single capture can contain several conversations using different protocol revisions, and it can also contain them sequentially. Storing the most recently observed version in a global makes one conversation's negotiation change the decoder used for another conversation and makes behavior depend on dissection order.

Merged master MR !14923 converts the World of Warcraft dissector to autogenerated message definitions and, as part of that work, moves protocol-version state into the per-conversation structure instead of keeping it globally. The submitted test capture deliberately contains every supported protocol version sequentially; the MR notes that the previous global version state made that capture decode incorrectly.

**Architecture rule:** if a version, mode, capability set, or negotiated dialect belongs to a connection/session, store it in state keyed by that connection/session. Do not use a mutable global merely because most ordinary captures contain only one protocol revision. Tests for version-sensitive dissectors should include multiple conversations or sequential sessions with different negotiated versions so cross-session leakage is visible.

**Confidence:** Very high. Merged master state correction exercised by a capture containing multiple protocol versions.

## Apply traffic-learned dissector-table registrations only on the first dissection pass

Some dissectors learn future dispatch mappings from packets—for example, a control or discovery message may reveal a port that should subsequently be registered with another dissector. Updating a dissector table is a persistent side effect, not presentation work for the current packet. Replaying that mutation every time the same frame is redisected makes global dispatch state depend on how often the UI, filters, or other consumers revisit a packet.

Merged MR !11253 adds SOME/IP DTLS auto-detection. During review, Pascal Quantin explicitly pointed out that calling `dissector_add_uint()` on every dissection of the packet would repeatedly mutate the table and required the registration to be guarded by `!PINFO_FD_VISITED(pinfo)`. The accepted diff uses exactly that first-pass guard before adding the learned `dtls.port` mapping. Merged MR !11254 applies the same rule to SOME/IP-SD's learned service-port registrations; its accepted diff registers those ports only when the frame has not already been visited, and Alexis La Goutte approved and merged it.

**Architecture rule:** when packet analysis discovers a persistent dissector-table or equivalent global dispatch registration that is intended to affect later packets, perform that registration only on the first sequential dissection pass. Redissection may consume the established mapping and may still perform ordinary packet presentation, but it must not replay the persistent registration side effect merely because the frame is being visited again.

**Review rule:** audit packet-driven calls to `dissector_add_*()` and similar registration APIs for first-pass semantics. If a learned mapping must instead vary by capture, conversation, or preference lifecycle, use a correspondingly scoped state mechanism rather than treating repeated redissection as an update signal.

**Confidence:** Very high. Two merged master changes use the same first-pass pattern, with Pascal Quantin explicitly identifying the redissection issue in review and Alexis La Goutte approving the companion SOME/IP-SD fix.

## Snapshot first-pass state at the PDU boundary for random-access redissection

A conversation's mutable "current" state is useful while the first sequential pass is learning a protocol state machine, but it is not sufficient evidence for later random-access dissection. By the time an older packet is revisited, the conversation object normally contains the state reached by later packets. If a single frame can contain multiple PDUs, a frame-level snapshot is not precise enough either: each PDU can begin in a different state.

Merged master MR !10762, authored and merged by John Thacker, fixes MySQL random-access dissection by keeping the evolving conversation fields only as first-pass working state and snapshotting the state needed to interpret each PDU into file-scoped protocol data. The snapshot includes the state, prepared-statement identifier, remaining field-packet count, and field metadata. It is keyed with the PDU's raw tvbuff offset so multiple PDUs in one frame remain distinguishable. Mutations of the live conversation state and creation of persistent prepared-statement data are guarded by \`!pinfo->fd->visited\`; later passes read the stored PDU snapshot instead of replaying the state machine.

**Architecture rule:** separate first-pass working state from redissection state. When packet interpretation depends on the sequential history of a conversation, persist the minimal state needed to interpret each semantic unit at the boundary where that unit begins. If several PDUs can share a frame, key the snapshot by a stable per-PDU identity such as its raw offset rather than by frame number alone.

**Redissection rule:** mutate the live learned state only on the intended first pass. Random access and later passes should consume the stored snapshot and must not require replaying all earlier packets to reconstruct the correct interpretation.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker, with the first-pass versus random-access contract documented directly in the accepted implementation.

## Version state mappings when protocol identifiers can be reused

An identifier that is unique only at one instant is not a sufficient key for file-scoped historical state. If a protocol can reuse the same channel, transaction, stream, or object ID later in the same connection, overwriting a single map entry destroys the mapping needed to dissect earlier packets correctly during random access.

Merged master MR !10685 fixes RDP dynamic virtual channels, where a server can reuse a channel ID after an earlier Create Channel attempt fails. The accepted implementation replaces a fixed one-entry-per-ID array with a file-scoped `wmem_multimap_t`: first-pass Create Channel packets insert a record keyed by channel ID and frame number, and later packet dissection uses `wmem_multimap_lookup32_le(..., pinfo->num)` to obtain the most recent assignment that existed at that point in capture history.

**Architecture rule:** when an on-wire identifier can be reused, include a generation/time dimension in the stored identity or retain versioned mappings keyed by a stable capture position. Lookup during redissection must select the mapping valid at the packet being dissected, not merely the newest mapping learned anywhere in the capture.

**Redissection rule:** create/version these mappings on the intended first pass and preserve the historical records needed for arbitrary packet access. A plain hash from reusable ID to "current object" is only correct if the protocol guarantees the ID cannot be reused within the state object's lifetime.

Merged master MRs !10665 and !10696 independently reinforce the same historical-state principle for MySQL: conversation state can hold the latest first-pass value, while per-frame/PDU snapshots provide the value that was valid at the packet being revisited.

**Confidence:** Very high. Merged master RDP fix accepted by Alexis La Goutte, independently corroborated by John Thacker's merged MySQL random-access fixes.
