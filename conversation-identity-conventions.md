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