# Wireshark Partial-Capture Inference Conventions

This file records durable conventions for inferring connection or role state when a capture begins after the underlying protocol exchange has already started. Current upstream source remains authoritative.

## Prefer direct protocol evidence, then use an explicit fallback heuristic

A capture is not guaranteed to begin with the first packet of a connection. Code that presents inferred endpoint roles or connection state must therefore avoid treating the first observed frame as proof of the protocol's actual first event. Use the strongest semantic evidence present in the observed packet, and only then fall back to a documented heuristic.

Merged MR !15535 fixes TCP Stream role selection. The old tap assumed that the first observed packet was a SYN and therefore treated its destination as the server. The accepted implementation examines TCP flags instead: a SYN without ACK identifies the destination as the likely server, a SYN-ACK identifies the source, and only when neither direct signal is available does it fall back to the lower-numbered-port heuristic. The UI still permits direction switching, acknowledging that the inference is useful rather than infallible.

**Implementation rule:** do not equate capture-start with conversation-start. When inferring roles or state from an incomplete conversation, rank evidence by protocol semantics first and apply heuristics only when stronger evidence is absent. Keep fallback behavior simple, deterministic, and easy to override where the UI or API permits it.

**Review rule:** test state/role inference with captures that begin at different points in a conversation, including SYN, SYN-ACK, established data, and atypical port assignments. A path that works only when the handshake is present is not robust to common midstream captures.

**Confidence:** Very high. Merged master change authored and merged by John Thacker with the old assumption, direct TCP evidence, and fallback policy explicitly described.