# Wireshark Conversation Identity Conventions

This file records durable rules for choosing conversation keys when protocol traffic does not map cleanly to a normal bidirectional transport endpoint tuple. Current upstream source remains authoritative.

## Key conversations by the protocol relationship that actually joins request and response

A normal address/port tuple is not always the identity of a logical exchange. Protocol topology can deliberately change one side of the tuple between request and response—for example, a multicast request followed by a unicast reply. In those cases, blindly using `find_or_create_conversation(pinfo)` can prevent related packets from sharing state even though the protocol itself gives a stable identity.

Merged MR !21653 carries John Thacker's LLMNR request/response tracking fix to release-4.6. RFC-defined LLMNR behavior sends UDP requests to a link-scope multicast address but requires responses to be unicast to the requester. The accepted implementation therefore constructs a custom conversation key from the requester's address and port plus `CONVERSATION_UDP`: request packets use the source endpoint, response packets use the destination endpoint. This lets both directions resolve to the same logical conversation despite the multicast/unicast address change.

**Architecture rule:** derive conversation identity from the protocol relationship that must retain state, not mechanically from the packet's full transport tuple. If request and response intentionally use different destination/source addressing, choose the stable subset or protocol identifier that is common to both directions. Conversely, do not broaden the key so far that unrelated logical sessions collide.

This complements the existing TFTP rule from merged !21803: endpoint tuples may also be *too broad* when reused by successive sessions. Together, the rules are: first identify the lifetime and identity of the protocol state, then choose or create a Wireshark conversation key that matches that identity.

**Confidence:** Very high. The LLMNR behavior is explicit in the merged implementation rationale, and the underlying master change was authored by John Thacker; the stable-branch backport was accepted and merged.