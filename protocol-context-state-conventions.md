# Protocol context state conventions

This file records durable conventions for protocol-owned context whose semantics are narrower than generic frame state or whose interpretation depends on a negotiated protocol mode. Current upstream source remains authoritative.

## Keep protocol-specific subunit state out of generic frame data

Generic per-frame structures are paid for by every packet and should not become a collection of protocol-specific scratch fields. If a field merely duplicates state that already belongs to one protocol's context, keep a single source of truth in the protocol-owned data and have consumers retrieve that context explicitly.

Merged master MR !14841, authored by John Thacker and approved/merged by Anders Broman, removes `frame_data::subnum`. RRC was the only remaining consumer, and the MR states that `fd->subnum` always duplicated the FP protocol data's `cur_tb`. The accepted change makes RRC retrieve the FP proto data and index its own state from `fpinf->cur_tb`; it also guards the lookup so the protocol-owned context is present before it is used. Removing the duplicate generic field also lets another small frame flag move next to the existing bitfields, reducing pressure on the per-frame structure.

**Architecture rule:** do not add a generic `frame_data` member for state whose identity and lifetime belong to one dissector or protocol layer. Prefer protocol-owned packet/file data and explicitly obtain the owning context at the point of use. This avoids divergent duplicate state, makes dependencies visible, and prevents protocol-specific needs from inflating every frame.

**Review rule:** when a proposed generic frame field has only one or a few protocol consumers, ask whether the value already exists in protocol data or can be carried there instead. If a consumer requires another protocol's context, handle the absence of that context rather than assuming the generic field will always have been populated.

**Confidence:** Very high. Merged master cleanup authored by John Thacker with the duplication and memory-footprint rationale stated directly and accepted by Anders Broman.

## Persist the negotiated mode when later common frames change meaning across drafts

Protocol evolution can reuse the same frame type while changing the identity or semantics of fields around it. If later packets cannot be interpreted correctly from their local wire shape alone, the dissector must remember the negotiated draft/mode on the connection and use that state when assigning identities or decoding shared frame types.

Merged master MR !14844, authored by John Thacker and approved/merged by Alexis La Goutte, updates QUIC multipath support for draft-07, where multipath identity changes from being tied to Connection ID sequence numbers to explicit Path IDs and multiple CIDs can belong to one path. During review, John supplied a new `multipath-07-decrypt.pcapng` sample and immediately identified that the first implementation broke the older multipath draft capture from issue #19314. The follow-up explicitly records whether multipath with or without Path ID was negotiated so ordinary `NEW_CONNECTION_ID` frames can be assigned the correct path semantics for the negotiated draft.

**Architecture rule:** when negotiation determines how later otherwise-shared frames are interpreted, make the negotiated mode part of conversation/session state. Do not infer the mode from whichever later frame happens to be under the cursor, and do not overwrite legacy semantics merely because a newer draft introduced a new identifier model.

**Testing rule:** version-sensitive dissector changes should exercise both the newly supported negotiation and previously supported captures. A new sample proving the new draft is insufficient if the same change alters the interpretation of common frames used by an older draft.

**Confidence:** Very high. Merged master compatibility fix authored by John Thacker, with explicit before-merge regression detection, a new sample, a legacy sample, and maintainer approval.

## Keep reverse-order, same-packet dependencies in packet-owned context

Some protocols place a discriminator or selector after the payload whose interpretation depends on it. When both values are in the same packet, the dissector can defer only the dependent portion of decoding until the later field arrives; it does not need process-global state or persistent file-scope storage.

Merged F1AP fixes !26576 (release-4.4) and !26575 (release-4.6), authored and merged by John Thacker, handle `UEContextRelease` where `RRCContainer` precedes the required `SRBID`. The accepted implementation stores the relevant subtree and TVB in `f1ap_private_data_t`, then completes the RRC decode when the later SRBID IE is seen. This also permits `f1ap_private_data_t` to return to `pinfo->pool` storage. The MR explicitly calls a dissector-level static `proto_tree *` an anti-pattern because proto trees are freed externally in `epan/proto.c`, and cites use-after-free failures from retaining them beyond their real lifetime.

**Architecture rule:** when a dependency is resolved later in the same packet, retain only the minimum deferred context in packet-owned protocol data and resume decoding when the selector arrives. Do not promote same-packet scratch state to static/global or file-scope lifetime merely to bridge field ordering.

**Lifetime rule:** `proto_tree *`, TVBs, and other packet-owned objects must not be retained in longer-lived static or global dissector state. The storage holding a pointer does not extend the pointee's lifetime; its owner must be scoped no broader than the object being referenced unless an explicit ownership mechanism says otherwise.

**Confidence:** Very high. Accepted fixes on two maintained stable branches, authored and merged by John Thacker, with the ordering and lifetime rationale stated directly in the MR.

## Validate predecessor-dependent state before publishing the current element

When parsing multiple logical protocol units that share a frame or another state container, the order in which the current unit is published can change what a lookup means. If a check is defined in terms of the previous unit, inserting the current unit into the searchable state first can make the lookup return the current unit itself and accidentally erase the predecessor invariant.

Merged master MR !14525, authored by John Thacker and merged by Alexis La Goutte, fixes QUIC early 1-RTT data from a server before handshake completion. For coalesced QUIC packets, the relevant check compares the current packet's Destination Connection ID with the preceding QUIC packet in the same frame. The accepted implementation delays allocating/inserting the current `quic_packet` state until after it looks up and validates the prior packet, so the shared packet list still denotes the predecessor when the check runs.

**Architecture rule:** when interpretation of the current element depends on previously published state, perform the predecessor lookup and validation before publishing the current element into the same searchable container. Treat insertion order as part of the state-machine contract rather than an incidental implementation detail.

**Review rule:** for same-frame/coalesced protocols, explicitly ask whether a helper named "last", "previous", or equivalent still returns the intended object after the current unit has been inserted. Tests should include early or otherwise atypical ordering that forces the predecessor-dependent path.

**Confidence:** Very high. Merged master correctness fix authored by John Thacker, with the accepted code ordering directly enforcing the required predecessor semantics.

## Publish newly discovered mapping state before later layers in the same packet need it

A dissector can discover identity or mapping information while decoding one layer and then need that information again in a higher layer of the *same* packet. If the new state is written only to the lookup path used by future packets, the first packet containing the information can still fail even though subsequent packets work.

Merged master MR !12866, authored and merged by John Thacker, fixes the first decryptable ZigBee APS packet. While decoding the network security header, Wireshark learns the extended source address from the nonce. The old path updated the IEEE-layer short-to-long mapping but did not immediately update the network-layer mapping consulted by APS; the first APS payload therefore missed information that later packets could find. The accepted change publishes the long address into the network-layer mapping as soon as it is learned, allowing the remaining layers of the current packet to consume it.

**Architecture rule:** when dissection discovers protocol state that a later layer in the current packet depends on, publish that state to the consumer's lookup domain before invoking or continuing that later layer. Do not rely on a future packet to make newly learned state visible.

**Review rule:** stateful dissectors should include a cold-start case where the first packet both establishes state and immediately consumes it. A trace that succeeds only from the second packet onward can hide an ordering bug in state publication.

**Confidence:** Very high. The bug and ordering rationale are stated directly in a merged master fix authored by John Thacker.
