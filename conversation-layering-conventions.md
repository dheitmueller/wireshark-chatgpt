# Wireshark Conversation-Layering Conventions

This file records durable conventions for conversation state when a dissector can run over multiple transports or when one protocol mediates another protocol's logical endpoints. Current upstream conversation APIs and dissector behavior remain authoritative.

## Do not create transport-specific state merely to derive transport-neutral facts

A shared application-layer dissector can be invoked over more than one transport. If it only needs a transport-neutral fact such as packet direction, it should not call a transport-specific state accessor whose side effect is to create or attach protocol state.

Merged master MR !11529, authored and merged by John Thacker, fixes HTTP after the dissector called `get_tcp_conversation_data()` to determine direction. HTTP can also be reached over UDP (for example SSDP), SCTP, and proxy-mediated paths; invoking the TCP helper in those cases created extra TCP streams and conversation data that did not correspond to a real TCP conversation. The accepted code derives direction from the packet's addresses and ports instead of instantiating TCP state solely for that purpose.

**Implementation rule:** distinguish pure observations from state-creating APIs. When a fact can be derived from `packet_info` or another transport-neutral input, do that rather than invoking a lower-layer accessor whose contract includes allocating, registering, or mutating conversation state.

**Review rule:** when an application dissector is reusable across transports, audit calls into transport-specific helpers for hidden side effects. Phantom Follow Stream entries, extra conversations, or state attached to the wrong transport can result even when the returned value itself appears correct.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker, with the unwanted TCP conversation/stream side effect identified directly in the MR.

## Keep a stateful mediator on the outer conversation across redissection

A protocol that rewrites logical endpoints or framing for a child protocol occupies a distinct layer in the conversation model. The outer transport conversation must continue to invoke the mediator on later dissection passes; the child protocol should be associated with the logical inner conversation after endpoint translation.

Merged master MR !11528, authored and merged by John Thacker, fixes two-pass dissection of PROXY protocol data when a TCP segment contains both a PROXY header and payload for a child protocol such as HTTP. The accepted implementation keeps the PROXY dissector bound to the outer TCP conversation, records proxied endpoints with `conversation_set_conv_addr_port_endpoints()`, and lets child dissection operate using the translated endpoint view. Without that layering, first-pass child dissection could bind the outer conversation directly to HTTP; on a later pass the PROXY dissector could then be skipped, losing both its framing work and the logical endpoint substitution.

**Layering rule:** if a dissector is responsible for transforming conversation identity, endpoint addresses/ports, or framing before dispatching a child protocol, keep that dissector as the owner of the outer conversation path. Attach or look up child-protocol state only after the mediator has established the inner logical endpoint context.

**Redissection rule:** conversation bindings must preserve the same protocol-layer sequence on every pass. Test coalesced packets and two-pass/redissection cases, because first-pass child bindings can otherwise bypass a mediator on later passes.

**Confidence:** Very high. Merged master two-pass conversation fix authored and merged by John Thacker, with the coalesced PROXY-header-plus-HTTP failure mode described directly in the MR.
