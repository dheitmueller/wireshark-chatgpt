# Wireshark Transaction-ID Reuse Conventions

## Preserve transaction history when a wire identifier may be reused

A map keyed only by a transaction or correlation identifier silently assumes that identifier is unique for the lifetime of the map. When the protocol permits reuse on the same connection, replacing the old entry destroys history and can associate a later response with the wrong request.

Merged MR !10820, authored and merged by John Thacker, changes Kafka request/response matching from a single-value `wmem_tree_t` keyed by correlation ID to a `wmem_multimap_t` that also records frame number. Lookup selects the applicable preceding instance, allowing the same correlation ID to be reused sequentially on one connection. The MR explicitly documents a remaining limitation: if an out-of-order capture contains multiple outstanding requests with the same ID before any response, correlation ID plus simple preceding-frame ordering is still insufficient.

**Implementation rule:** when a transaction identifier is reusable, use a state structure capable of representing multiple historical instances rather than overwriting one value per identifier. Choose an ordering or secondary key that matches the protocol/capture semantics; if duplicate IDs can overlap concurrently, add enough independent context to distinguish those transactions instead of assuming nearest-preceding order is always authoritative.

**Testing rule:** include sequential ID reuse, and—when the protocol permits it—overlapping or out-of-order duplicate-ID cases so the limits of the chosen correlation key are explicit.

**Confidence:** Very high for the sequential-reuse rule. The master correction was authored and merged by John Thacker and states both the accepted multimap design and its known ambiguity boundary.
