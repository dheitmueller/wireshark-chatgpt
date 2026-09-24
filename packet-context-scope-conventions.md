# Wireshark Packet-Context Scope Conventions

This file records durable conventions for deciding what belongs in ubiquitous packet context versus protocol-owned state. Current upstream source remains authoritative.

## Keep protocol-specific convenience state out of ubiquitous packet context

A field added to `packet_info` is shared across packet dissection and imposes a cost on every packet context. Protocol-specific convenience data should therefore normally remain with the protocol that owns it.

Merged MR !10964 initially added an HTTP/2 `:path` pointer to `packet_info` so JSON 3GPP could decode feature negotiation. Pascal Quantin explicitly objected to increasing the `packet_info` memory footprint for an HTTP/2-specific use case. He first suggested `p_add_proto_data()` / `p_get_proto_data()`, then pointed out that HTTP/2 already exposed `http2_get_header_value()`, which gRPC used for the same header. Merged follow-up !10995 removes the `packet_info` member and has JSON 3GPP query the HTTP/2 accessor directly, checking the opposite direction when required.

**Architecture rule:** before extending a ubiquitous context structure such as `packet_info`, ask whether the datum is genuinely protocol-neutral and broadly needed. If it belongs to one protocol or protocol relationship, keep it in protocol/session state and expose a narrow accessor or protocol-scoped data mechanism instead. This avoids per-packet memory cost and global API coupling for local concerns.

**Review rule:** protocol-local accessors are preferable to a new shared-context member, but their side effects still matter. Later merged MR !15023 showed that `http2_get_header_value()` can create HTTP/2 session state, so speculative callers must first establish that HTTP/2 is actually present.

**Confidence:** Very high. Direct Pascal Quantin architectural review on a merged MR was followed by a merged corrective implementation that removed the shared-context field.
