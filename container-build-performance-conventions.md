# Wireshark Container Construction Performance Conventions

This file records durable conventions for choosing list/container operations when building collections in Wireshark. Current upstream APIs and source remain authoritative.

## Avoid accidental quadratic list construction

Repeatedly appending to a singly linked list whose append operation must rediscover the tail turns an otherwise linear build into quadratic work. Choose the operation and container from the ordering requirement rather than using append by habit.

Merged master MR !11421 improves Protobuf language-file loading when many `.proto` files are present. The old code repeatedly used `g_slist_append()`, which calls `g_slist_last()` and traverses the list for every insertion. The accepted implementation uses `g_slist_prepend()` where order is irrelevant and changes order-sensitive collections to `GQueue`, which supports efficient tail insertion.

**Implementation rule:** when constructing a collection incrementally, account for the complexity of the insertion operation. If order does not matter, constant-time prepend is usually preferable for a singly linked list. If insertion order must be preserved and tail insertion is frequent, use a container with an explicit tail rather than repeatedly walking a singly linked list.

**Review rule:** performance review should look for complexity changes that only become visible on large configuration/schema/plugin sets. A loop containing an apparently small list append can dominate startup when the append itself is O(n), producing O(n²) total work.

**Confidence:** High. Merged master performance fix with the GLib list traversal cost and the order-sensitive/order-insensitive replacement strategy stated explicitly in the MR rationale.
