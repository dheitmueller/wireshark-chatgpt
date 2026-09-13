# Wireshark wmem Container Lifetime Conventions

This file records durable ownership and lifetime rules for wmem containers extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Treat allocator lifetime as the normal destruction mechanism for wmem containers

A `wmem_map_t` is not a standalone heap object whose complete ownership graph can be released correctly with a single `wmem_free()` call. Its implementation can contain separately allocated tables, item arrays, callback state, and other allocator-managed storage. Freeing only the map object can therefore leave internal storage or callbacks behind, while bypassing the normal allocator lifecycle.

Merged MR !20650, authored and merged by John Thacker, fixes RTP code that directly freed a wmem map and instead lets the allocator perform normal cleanup. Merged !20651 makes the intended ownership model explicit in the public API documentation: wmem maps are not really intended for NULL allocator scope, and callers that need manually managed lifetime should normally use `GHashTable` instead. Merged !20656 adds `wmem_map_destroy()` for the exceptional case where explicit early destruction is required, while warning that needing it can indicate that the code should instead use a better allocator/lifetime design. Merged !20657 then converts callers that truly need early destruction to the dedicated container API rather than freeing only the map object. The preceding merged !20647 also ensures non-wmem auxiliary storage owned by the map implementation is released from the allocator callback.

**Implementation rule:** normally choose a wmem allocator whose lifetime matches the container and let allocator reset/destruction clean up the container. Never call `wmem_free()` on a wmem container as though that necessarily destroys all of its internal state. If early destruction is genuinely required, use the container's dedicated destruction API and understand its key/value ownership semantics. If the desired model is ordinary manually freed storage or a NULL allocator, prefer a container designed for that ownership model, such as `GHashTable`.

**Review rule:** repeated explicit destruction of scope-managed containers is a signal to reconsider the allocation scope or data-structure choice. Audit container metadata, backing arrays, keys, values, callbacks, and any non-wmem side allocations rather than checking only the top-level pointer.

**Confidence:** Extremely high. A concentrated series of merged master changes authored by John Thacker establishes both the implementation fix and the public API guidance, with Gerald Combs also accepting the callback-lifetime correction.