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


## Keep search wildcards separate from creation-side omitted endpoints

Merged core MR !7064 separates conversation option domains that were previously easy to misuse. `find_conversation()` uses `NO_ADDR_B` and `NO_PORT_B` for wildcard query arguments, while `conversation_new()` uses `NO_ADDR2` and `NO_PORT2` for missing parts of the stored second endpoint. The core API adds assertions for the distinction. Stig Bjørlykke noted that existing callers also needed checking, and merged follow-ups !7100, !7101, !7103, !7104, !7105, and !7106 repair real callers.

**Rule:** lookup options and creation options are separate semantic domains even though both use integer flags. Audit callers whenever the core flag contract changes.

Merged MR !7089 also normalizes null address arguments to `AT_NONE` at the `find_conversation()` boundary. Merged !7108 removes a downstream null check that became impossible after that normalization.

**Rule:** normalize nullable inputs once at a documented API boundary and let internal code rely on that invariant.

**Confidence:** Very high. Merged conversation-core work with direct Stig Bjørlykke review and multiple merged caller corrections.


## Use typed element lists when conversation identity is not an address/port tuple

Merged core MR !6979, authored by Gerald Combs, adds `conversation_new_full()` and `find_conversation_full()` for identities described by arbitrary typed element lists. Supported elements include addresses, strings, unsigned integers, and unsigned 64-bit integers, with an endpoint-type terminator. Falco Bridge immediately uses this to describe protocol-specific identity without pretending it is a transport tuple.

The accepted implementation deep-copies the retained element list and nested address/string values into file-scope storage before inserting the key into persistent conversation maps. Merged !7001 then migrates the existing by-ID conversation helpers onto the same element-list machinery, expands their identifiers to 64 bits, and removes tuple-option arguments that those ID-only callers never used.

**Identity rule:** choose conversation keys from the stable semantic identifiers the protocol actually uses. When address/port tuples are not the identity, use the typed conversation-element mechanism rather than manufacturing fake endpoints.

**Ownership rule:** conversation keys outlive the packet that established them. Any address/string storage retained by the key must be copied into a compatible long-lived scope.

**Confidence:** Very high. Two adjacent merged conversation-core changes authored by Gerald Combs, with the second explicitly migrating older special-case identity onto the generalized model.

## Similar conversation flags may differ in later lifecycle behavior

Closed MR !6901, authored by Gerald Combs, proposed removing `NO_PORT2_FORCE` on the assumption that it was equivalent to `NO_PORT2`. The two flags selected the same no-port2 conversation table in `conversation_new()`, but they differed in `conversation_set_port2()`: `NO_PORT2_FORCE` deliberately prevented later specialization of the wildcarded port. Gerald closed the MR after tracing that difference.

**API rule:** do not collapse conversation flags merely because they lead to the same lookup table or creation path. Audit subsequent mutation, specialization, template handling, and lookup semantics across the full conversation lifecycle.

**Review rule:** when simplifying flag domains, search every consumer of the flag, especially setters and state-transition helpers. Equivalent construction is not proof of equivalent behavior.

**Confidence:** Very high as negative evidence. The core API maintainer authored the simplification and then explicitly closed it after discovering the semantic distinction.

