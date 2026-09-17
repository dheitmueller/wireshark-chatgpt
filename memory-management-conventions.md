# Wireshark Memory-Management Conventions

This file records durable memory-management lessons extracted from upstream Wireshark merge-request review. Current upstream source remains authoritative.

## Match allocation and deallocation domains

- Free an allocation with the allocator family and scope that created it. Do not mix GLib allocation/free routines with `wmem` allocations merely because both ultimately manage heap memory.
- Merged MR !17630 is a concrete failure case: UMTS RLC fragment buffers had been moved to `wmem_file_scope()` to avoid leaks from malformed-packet state, but cleanup still used `g_free()`. That could result in complaints at file close, freeing the middle of a wmem-managed block, or double-free behavior under `FORTIFY_SOURCE`. The accepted fix uses `wmem_free(wmem_file_scope(), ...)` for those buffers.
- Treat allocator migration as a lifecycle change, not a mechanical call-site change. When changing an object's allocation domain, audit every destruction/cleanup path and any container callbacks that own it.
- Even a zero-length memory operation must obey the C API's pointer preconditions. MR !17630 also avoids calling `memcpy()` with a NULL source even when the length is zero, because that is undefined behavior under the C standard and can be diagnosed by sanitizers/toolchains.

## Container deletion semantics

- A data structure's removal operation must satisfy the lookup semantics promised by all of its APIs. Merged MR !17658 replaced wmem tree's prior "fake removal" for 32-bit keys with actual red-black-tree deletion because leaving a tombstoned node in place broke predecessor-style `wmem_tree_lookup32_le()` lookup: the removed node could be found without backtracking to the next valid node.
- When modifying a core container, test exact lookup, ordered/predecessor lookup, removal, and structural rebalancing rather than validating only the most obvious access path.
