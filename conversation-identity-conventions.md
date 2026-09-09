# Wireshark Conversation Identity Conventions

This file records durable rules for conversation identity and dispatch extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Give protocol shims and proxies their own conversation identity when dispatch semantics differ

A conversation type is not merely a label for the underlying transport. It participates in lookup and dissector attachment, so reusing an existing transport conversation type for a shim/proxy can collide with conversations owned by other dissectors and can even route the proxy back into itself.

Merged MR !17572 (`PROXY: Use a CONVERSATION_PROXY type`) adds a dedicated conversation type for the PROXY protocol. The change explains that the PROXY dissector rewrites source/destination information and then attaches the target dissector to a conversation; reusing the underlying TCP/SCTP type could find an unrelated existing conversation (for example one already associated with HTTP) or recursively invoke PROXY in pathological cases.

**Architecture rule:** when an encapsulation, proxy, or shim layer changes how a flow is identified or how the next dissector is attached, model that distinction explicitly with its own conversation identity/type rather than borrowing the underlying transport's conversation type. Conversation keys and types must represent the logical state/dispatch domain that owns the conversation.

**Confidence:** Very high. Merged master architecture fix; the failure mode is intrinsic to conversation lookup/attachment semantics rather than protocol-specific style.
