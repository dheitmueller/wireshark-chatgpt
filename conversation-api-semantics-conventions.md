# Wireshark Conversation API Semantics

## Name conversations, endpoints, and identity elements by their real roles

Merged master MR !7931, authored by Guy Harris, makes a core distinction explicit: a Wireshark conversation is not an endpoint. A conversation can have two endpoints or none, so conversation-type APIs and endpoint-type APIs should not share misleading terminology merely because both participate in flow identity.

Merged master MR !7934, also authored by Guy Harris, renames helpers from “create key” to “set elements”. They do more than allocate an element array: they install that identity into `packet_info`, which can change how downstream dissectors find or create conversations.

**Rule:** API names should describe both the semantic object and material side effects. Treat conversation identity installed in `packet_info` as transient dissection context, not a passive local key.
