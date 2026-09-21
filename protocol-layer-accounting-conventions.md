# Wireshark Protocol-Layer Accounting Conventions

This file records durable conventions for `packet_info` protocol-layer bookkeeping when dissectors may be reinvoked, desegment, or contribute no bytes on a particular pass. Current upstream source remains authoritative.

## Keep layer identity stable across first-pass and redissection paths

A dissector invocation is not necessarily equivalent to a semantic protocol layer that should remain in `pinfo->layers`. Desegmentation and reassembly paths can invoke a dissector, return a nonzero value, and still consume no bytes or add no meaningful protocol-tree contribution on that pass. If such an invocation permanently increments layer bookkeeping, a later PDU in the same frame can receive a different layer number on the first pass than it receives during redissection. Any file-scoped or reassembly state keyed by that layer number can then fail to match.

Merged master MR !14106, authored by John Thacker, fixes this in the generic dissector path. A dissector can set desegmentation state at offset zero and return nonzero while actually consuming no bytes; when that happens on the first pass, the temporary protocol-layer entry is removed before processing continues. Peter Wu's review also highlights an important precision requirement: rollback must remove the layer belonging to the non-consuming invocation, not an unrelated fallback such as the data dissector.

Merged master MR !14101, also authored by John Thacker, provides the concrete reassembly failure that motivated the general rule. With multiple TLS records in one frame, an HTTP/2 fragment could cause TLS to acquire a layer number on the first pass even though that invocation added nothing to the tree. The invocation pattern differed on later passes, so the corresponding layer number changed and HTTP/2 reassembly state could no longer be found reliably.

**Implementation rule:** protocol-layer bookkeeping should reflect a real, retained protocol contribution, not merely the fact that a dissector function was entered or returned a nonzero value. If an invocation contributes no bytes and is not guaranteed to recur identically during redissection, roll back any temporary layer entry before dispatch continues.

**State rule:** do not key persistent/reassembly state on a layer number whose first-pass allocation can be changed by no-consumption desegmentation or heuristic attempts. Either make the layer accounting stable or use a more semantically stable identity.

**Review/testing rule:** for changes involving desegmentation, reassembly, or multiple PDUs per frame, exercise both the initial dissection and redissection paths. Include captures where an early fragment consumes no data before a later PDU in the same frame, and verify that layer numbers and layer-keyed state remain stable.

This is distinct from semantic nesting depth. The guidance in `protocol-nesting-and-token-boundary-conventions.md` explains that true nested instances of the same protocol should use explicit semantic depth rather than generic invocation order. The rule here is that generic layer accounting itself must also avoid recording transient no-contribution invocations that make later identity unstable.

**Confidence:** Very high. Both principal fixes were merged to master and authored by John Thacker; !14106 generalized the behavior after !14101 exposed the concrete cross-pass reassembly failure, with additional review by Peter Wu.
