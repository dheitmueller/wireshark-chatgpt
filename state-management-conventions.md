# Wireshark State-Management Conventions

This file records durable state lifetime, persistence, and mutation-order rules extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Do not let short-lived allocator state escape into longer-lived protocol state

State stored in conversation, file, or other persistent dissector data must be allocated from a scope that lives at least as long as the state that references it. Packet-scope objects such as data allocated from `pinfo->pool` cannot safely be inserted into file-scope conversation state and then retrieved by a later frame.

Merged MR !21063 (`HTTP: corrected memory management and tracking for header_value_map`) is direct evidence. John Thacker identified a path where an HTTP header map could be allocated with `pinfo->pool`, later stored in `wmem_file_scope()` conversation data, and then retrieved after the packet pool had been freed. He also pointed out the opposite failure mode: promoting all HTTP header maps to file scope causes unnecessary persistent growth for ordinary requests and responses that never need the headers later.

**Implementation rule:** choose the narrowest lifetime that satisfies actual persistence requirements, but every object reachable from longer-lived state must itself have a compatible lifetime. If whether data must persist is only known after parsing, it is often better to parse in a short-lived scope and copy/promote the needed result when persistence becomes necessary than to allocate every candidate in file scope up front.

**Review rule:** when a pointer is stored in packet proto data, conversation state, file state, or another container, trace the allocator scope of the object and of any keys/values it owns. Check both use-after-free risk from too-short a scope and long-session memory growth from an unnecessarily broad scope.

**Confidence:** Very high. Merged master correctness change with detailed direct review from John Thacker and final merge by John.

## Wiretap private state owns its nested allocations explicitly

For a Wiretap reader, `wth->priv` is a convenient per-file state root, but Wiretap only directly frees the object referenced by `wth->priv` itself. If that private structure contains pointers to separately allocated memory, the format handler is responsible for releasing those nested allocations.

Merged MR !21054 updates the Wireshark Developer's Guide to make this contract explicit: persistent per-file data should be referenced from `wth->priv`; the directly referenced object is automatically freed with `g_free()` when the capture closes; separately allocated objects reachable through that structure require a `subtype_close` routine that frees them.

**Implementation rule:** treat `wth->priv` as an ownership root, not as recursive automatic cleanup. When adding pointer members to a Wiretap private structure, define who frees each pointee and provide `subtype_close` whenever nested allocations require explicit cleanup.

**Confidence:** Very high. Merged documentation of Wiretap's intended ownership contract, accepted by Michael Mann.

## Derive state transitions from the old state before overwriting it

When a state-machine update depends on the difference between the previous and new values, capture or consume the previous value before assigning the replacement. Updating the canonical state too early can silently collapse the computed transition to zero or otherwise destroy the information needed to update dependent state.

Merged MR !21066 (`HTTP2: fix updating of existing streams' window`) fixes exactly this ordering bug. HTTP/2 code assigned the new initial window size and only afterward computed the delta from the stored value, making the delta always zero. The accepted fix computes and applies the delta first, then stores the new initial value. John Thacker approved and merged the change.

**Implementation rule:** for state transitions of the form `delta = new - old`, preserve `old` until every dependent update has consumed it. Review adjacent assignments when state-machine arithmetic unexpectedly produces identity/zero transitions.

**Confidence:** Very high. Merged master correctness fix approved and merged by John Thacker.
