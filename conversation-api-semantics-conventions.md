# Wireshark Conversation API Semantics

## Name conversations, endpoints, and identity elements by their real roles

Merged master MR !7931, authored by Guy Harris, makes a core distinction explicit: a Wireshark conversation is not an endpoint. A conversation can have two endpoints or none, so conversation-type APIs and endpoint-type APIs should not share misleading terminology merely because both participate in flow identity.

Merged master MR !7934, also authored by Guy Harris, renames helpers from “create key” to “set elements”. They do more than allocate an element array: they install that identity into packet_info, which can change how downstream dissectors find or create conversations.

**Rule:** API names should describe both the semantic object and material side effects. Treat conversation identity installed in packet_info as transient dissection context, not a passive local key.

## Read-only consumers should not create conversation state

Merged master MR !7595, authored by John Thacker, changes TCP/UDP/DCCP/HTTP2 Follow Stream filter construction to find an existing transport conversation instead of calling helpers that may create conversations or allocate new stream numbers.

**Rule:** UI, lookup, and filter-generation paths that are only inspecting prior dissection state should use non-creating conversation lookup. New conversation identity and stream numbering belong to packet processing, where protocol state is established deliberately.

The same change also checks the transport endpoint type explicitly. A packet can carry multiple protocol layers, so a generic packet-info lookup must not accidentally return a conversation from another identity domain.

**Confidence:** Very high. Merged master correctness change authored by John Thacker.

## Lookup and creation must use the same identity rule

Merged master MR !7515, authored by John Thacker, fixes find_or_create_conversation after endpoint-element support was added. The lookup path honored pinfo->use_endpoint / pinfo->conv_elements, but the create path still used ordinary src/dst/transport endpoints, so a later call could fail to find the conversation that the previous call had just created.

**Rule:** A find-or-create API must use one canonical key derivation for both halves. If lookup accepts alternate endpoint or element identity, creation must persist state under that same identity.

**Confidence:** Very high. Merged master conversation-core correctness fix authored by John Thacker.

## Follow Stream should reuse protocol-resolved identity

Merged master MR !7530, authored by John Thacker, changes QUIC Follow Stream filter construction to retrieve the stored per-packet QUIC datagram/connection record. Reconstructing the connection from addresses and ports is unreliable because QUIC supports migration and because distinct connections may reuse a UDP 5-tuple.

**Rule:** When dissection has already resolved a protocol's canonical connection identity, read-only UI/filter consumers should retrieve that stored identity rather than recomputing it from transport coordinates.

**Confidence:** Very high. Merged master correctness fix authored by John Thacker.
