# Wireshark Conversation Identity Conventions

This file records durable rules for choosing conversation keys when protocol traffic does not map cleanly to a normal bidirectional transport endpoint tuple. Current upstream source remains authoritative.

## Key conversations by the protocol relationship that actually joins request and response

A normal address/port tuple is not always the identity of a logical exchange. Protocol topology can deliberately change one side of the tuple between request and response—for example, a multicast request followed by a unicast reply. In those cases, blindly using `find_or_create_conversation(pinfo)` can prevent related packets from sharing state even though the protocol itself gives a stable identity.

Merged MR !21653 carries John Thacker's LLMNR request/response tracking fix to release-4.6. RFC-defined LLMNR behavior sends UDP requests to a link-scope multicast address but requires responses to be unicast to the requester. The accepted implementation therefore constructs a custom conversation key from the requester's address and port plus `CONVERSATION_UDP`: request packets use the source endpoint, response packets use the destination endpoint. This lets both directions resolve to the same logical conversation despite the multicast/unicast address change.

**Architecture rule:** derive conversation identity from the protocol relationship that must retain state, not mechanically from the packet's full transport tuple. If request and response intentionally use different destination/source addressing, choose the stable subset or protocol identifier that is common to both directions. Conversely, do not broaden the key so far that unrelated logical sessions collide.

This complements the existing TFTP rule from merged !21803: endpoint tuples may also be *too broad* when reused by successive sessions. Together, the rules are: first identify the lifetime and identity of the protocol state, then choose or create a Wireshark conversation key that matches that identity.

**Confidence:** Very high. The LLMNR behavior is explicit in the merged implementation rationale, and the underlying master change was authored by John Thacker; the stable-branch backport was accepted and merged.

## Give higher-layer sessions their own stable stream identity when transport identity is insufficient

Features such as Follow Stream should key themselves to the logical session being followed, not infer identity from whichever lower-layer transport happens to carry the current packet. This matters when the same protocol can be carried by different transports, nested in other sessions, or otherwise outlive a simple TCP/UDP port-type test.

Merged MR !21008, authored and merged by John Thacker, adds an explicit generated TLS stream identifier allocated when a TLS session is created and uses TLS session identity in the follow-stream path instead of relying on the packet's final port type. QUIC handshake-only TLS is deliberately excluded because it does not represent a standalone followable TLS byte stream. Review also ensured repeated dissector-handle lookup was considered in terms of its actual per-session frequency rather than assumed per-packet cost.

**Architecture rule:** when a user-facing or analytical feature operates on a higher-layer protocol session, assign and persist a protocol-layer identity with the same lifetime and semantics as that session. Do not substitute a lower-layer transport discriminator merely because it usually correlates with the protocol; explicitly handle protocol modes that do not form the same kind of stream.

**Confidence:** Extremely high. Merged master architecture authored and merged by John Thacker, with review centered on how the TLS session is created and reused.

## Include every semantic discriminator in lookup and cache keys

A numeric identifier is not a complete key when the same number legitimately names different objects in different semantic domains. Caches must distinguish all dimensions that affect the lookup result; otherwise a previous lookup can poison later lookups for another domain, including by caching a negative result.

Merged master MR !20539, authored by John Thacker and merged by Anders Broman, fixes service-name resolution by keying the cache with both the port number and `port_type`. TCP, UDP, SCTP, and DCCP can use the same numeric port for different services, and the old cache could retain a NULL/result from one transport and incorrectly reuse it for another.

**Architecture rule:** before choosing a cache/state key, enumerate the complete semantic identity of the object being cached. If a lookup result varies with transport type, direction, namespace, protocol mode, tag-owner bit, interface, or another discriminator, that discriminator belongs in the key even when one component is usually sufficient on common traffic. Cached misses require the same complete key discipline as cached hits.

**Confidence:** Extremely high. Merged master correctness fix authored by John Thacker, consistent with the notebook's existing reassembly/conversation identity rules.

## Reassociate established protocol state when the transport conversation legitimately changes

A stateful protocol can retain one logical connection while the observed address/port tuple changes. Once Wireshark has enough protocol evidence to identify the established connection, the newly observed transport conversation must be associated with that existing state so packets in the reverse direction can find the same connection even when those packets carry no independently useful protocol identifier.

Merged master MR !13702 fixes QUIC connection migration and NAT rebinding, including the difficult zero-length connection-ID case. After a client packet from the new address is recognized as belonging to an existing QUIC connection, the accepted implementation associates that connection with the new Wireshark conversation so the server's replies to the new tuple can be found and decrypted. John Thacker's substantive review distinguished prohibited connection-ID reuse during active migration from RFC-permitted NAT rebinding; the merged solution handles the legitimate address-change case rather than assuming every tuple change means a new QUIC connection.

The closed predecessor !13693 is useful testing evidence even though it is not implementation precedent: the contributor supplied a concrete migration capture plus key log and identified the first migrated packet and later packets that should decrypt. That is the right shape of regression artifact for a state-association bug whose symptom occurs several packets after the causal tuple change.

**Architecture rule:** if the protocol explicitly permits migration or rebinding, treat the transport tuple as a location for the established state, not necessarily its identity. When protocol-level evidence resolves a packet to an existing connection, attach the new conversation to that connection before subsequent packets depend on tuple-only lookup.

**Testing rule:** test both directions across the endpoint change, especially a reverse-direction packet that cannot identify the connection by itself. Include a real migrated/rebound capture and any required decryption secrets, and verify packets after the migration rather than only the first packet that announces or reveals the new path.

**Confidence:** Very high. Merged master fix with detailed John Thacker review and concrete capture/keylog reproduction from its superseded predecessor; it also corroborates the notebook's existing reassembly rule that protocol session identity can outlive endpoint tuples.

## Scope protocol sequence/timestamp state by the protocol-defined namespace, not merely the transport session

A transport conversation can contain several independent protocol state spaces. When the specification defines a discriminator that owns its own sequence numbers, timestamps, counters, or generations, persistent state must be keyed by that discriminator even when all of those streams share one address/port tuple.

Merged master MR !11493, authored and merged by John Thacker, fixes RTP extended sequence-number and timestamp tracking. RTP defines each SSRC as its own timing and sequence-number space, but the old conversation data retained only one most-recent sequence number/timestamp pair for the whole RTP conversation. Multiple SSRCs on one 5-tuple could therefore make one stream appear to wrap or jump based on another stream's history. The accepted implementation stores that state per SSRC while packet-local data continues to contain only the values relevant to the packet being dissected. The same change also improves 5-tuple reuse because a newly observed SSRC starts in its own number space rather than inheriting cycle state from an older sender.

**Architecture rule:** distinguish the identity of the enclosing Wireshark conversation from the identity of each state namespace inside it. If the protocol says a field such as SSRC, channel ID, stream ID, generation, or direction defines an independent sequence/timestamp/counter space, include that field in the state key rather than treating the enclosing transport tuple as sufficient identity.

**Review rule:** when code keeps a “last”, “highest”, cycle count, rolling timestamp, or similar history at conversation scope, ask whether every packet in that conversation is normatively part of the same number space. Multi-stream captures and reuse of a familiar 5-tuple are useful tests for state-key collisions.

**Confidence:** Extremely high. Merged master correctness fix authored and merged by John Thacker, with the protocol namespace requirement stated directly from RTP/RFC semantics.

## Configure conversation endpoints without cloning packet_info

A dissector may need synthetic endpoints or port discriminators for a logical conversation (including TLS carried inside EAP), but copying a whole `packet_info` and modifying that copy can desynchronize other packet metadata and later consumers. **Merged release-4.0 !9373**, authored by **Guy Harris**, replaces EAP's temporary `packet_info` clone and private address/port mutations with `conversation_set_conv_addr_port_endpoints()` on the actual `pinfo`. The same packet continues through TLS/application subdissectors while the conversation endpoint layer carries the EAP identity and nested protocol-layer discriminator.

**Architecture rule:** use the explicit conversation-endpoint API for alternate logical identity; do not manufacture a shallow packet-info copy just to affect conversation lookup. Check when the endpoint override is visible to subsequent dissectors and final packet metadata, especially alongside the !9914 final-context contract. **Confidence:** extremely high for Guy Harris's merged correction; !9373 is a stable backport of an earlier master fix.

## A transport five-tuple need not identify one application session

Merged master !9141 (John Thacker) handles multiple QUIC connections multiplexed over one UDP five-tuple. A QUIC connection has its own connection-ID namespace and may migrate between UDP paths; using the UDP conversation as its sole identity merges unrelated sessions. The accepted implementation adds CONVERSATION_QUIC, maintains a chain of connection records for the same tuple, checks the relevant CIDs and Stateless Reset tokens across them, and isolates child TLS state in the logical QUIC conversation. The MR discussion identifies capture-based checks for both multiplexing and migration.

**Rule:** assign flow identity at the layer whose specification defines it, not by reflexively reusing an enclosing transport tuple; test multiplexed sessions, migration and resets as separate cases. **Confidence:** very high (John Thacker's merged implementation and tested reproducers).

## Give all child-flow creation paths the protocol discriminator needed for reassembly

Merged master !9115 and stable backport !9128 (John Thacker) ensure NBAP HS-DSCH bindings created by RadioLinkReconfiguration also carry CommunicationContextID/UEID. Omitting that discriminator in just one creation path allows unrelated radio flows to collide in RLC reassembly despite correct lookup elsewhere.

**Rule:** audit every constructor/handoff that creates the same logical flow; its identity fields must be initialized consistently, including uncommon control-plane transitions. **Confidence:** high (accepted master and backport).

## Treat endpoint IDs as address objects when the protocol defines its own flow namespace

Merged master !9054 makes BPv7 source/destination Endpoint IDs first-class `address` values, introduces `CONVERSATION_BP`, and reshapes bundle identity APIs and Debian symbol metadata. Bundle endpoint identity is not necessarily a TCP/UDP address pair; using the protocol's own typed address avoids forcing a network-tuple identity onto a different namespace. **Evidence limit:** the corpus omits the large `packet-bpv7.c` diff, so specific internal lookup mechanics were not independently established by this review. **Rule:** model the protocol's actual endpoint identifier and audit public structure/API changes and exported packaging metadata together. **Confidence:** high for the visible accepted header/enum/API change; narrower for unseen implementation details.
