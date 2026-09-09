# Wireshark Conversation Identity Conventions

This file records durable rules for conversation identity and dispatch extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Give protocol shims and proxies their own conversation identity when dispatch semantics differ

A conversation type is not merely a label for the underlying transport. It participates in lookup and dissector attachment, so reusing an existing transport conversation type for a shim/proxy can collide with conversations owned by other dissectors and can even route the proxy back into itself.

Merged MR !17572 (`PROXY: Use a CONVERSATION_PROXY type`) adds a dedicated conversation type for the PROXY protocol. The change explains that the PROXY dissector rewrites source/destination information and then attaches the target dissector to a conversation; reusing the underlying TCP/SCTP type could find an unrelated existing conversation (for example one already associated with HTTP) or recursively invoke PROXY in pathological cases.

**Architecture rule:** when an encapsulation, proxy, or shim layer changes how a flow is identified or how the next dissector is attached, model that distinction explicitly with its own conversation identity/type rather than borrowing the underlying transport's conversation type. Conversation keys and types must represent the logical state/dispatch domain that owns the conversation.

**Confidence:** Very high. Merged master architecture fix; the failure mode is intrinsic to conversation lookup/attachment semantics rather than protocol-specific style.

## Snapshot layer-local stream identity at the point where tap data is produced

`packet_info` is mutable across the dissection stack. By the end of a frame, its addresses and ports can describe a later encapsulation layer rather than the transport layer that emitted a tap event. Consumers such as Follow Stream therefore must not reconstruct the earlier layer's identity from the final `packet_info` state.

Merged MR !25426, authored and merged by John Thacker, changes TCP Follow Stream to use the addresses and ports saved in the TCP header/tap data instead of `pinfo->{src,dst}` and `pinfo->{src,dst}port`. Merged MR !25450 applies the same principle to UDP and DCCP by using `follow_stream_tap_listener`, which captures addresses, ports, and stream ID at tap time. Both changes explicitly cite tunneling as the failure mode: later dissectors can rewrite packet identity, causing follow data to be assigned to the wrong stream or endpoint.

**Implementation rule:** when a tap, reassembly helper, or deferred consumer needs the identity of a particular protocol layer, capture that identity when the layer is being dissected and carry it with the event/state. Do not later infer it from mutable frame-global `packet_info` fields whose values may have been changed by inner or outer dissectors.

**Confidence:** Extremely high. Two independent merged master fixes authored by John Thacker, covering TCP and UDP/DCCP and documenting the same tunneling failure mode.
