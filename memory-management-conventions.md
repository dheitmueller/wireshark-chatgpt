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

## Size allocations from the object actually being allocated

Pointer depth is part of an allocation contract. A `sizeof` expression taken at the wrong indirection level can accidentally produce the expected byte count on today's platforms when the two candidate types are both pointers, while still expressing the wrong object and becoming fragile if the type shape changes.

During review of merged MR !16393, John Thacker relayed Coverity's warning that an allocation through a pointer-to-pointer-to-pointer parameter should use the size of the actual array element. Guy Harris emphasized that `sizeof *X` and `sizeof **X` only happened to be equal because all relevant pointer representations have the same size on supported platforms; he explicitly said that should not be relied upon and suggested `g_new()`. Guy also suggested `GPtrArray` because the result was a variable-sized array of pointers. The author acknowledged that an earlier pointer type had made the stale `sizeof` expression once correct and followed up in merged !16407 by converting the candidate collection to `GPtrArray`.

**Implementation rule:** derive allocation size from the type of the element actually stored, preferably with a typed allocator such as `g_new()` when using GLib allocation. Do not preserve a `sizeof *ptr` expression mechanically after changing pointer depth just because the resulting numeric size is currently identical.

**Container rule:** when the object is semantically a variable-sized collection of pointers, consider a pointer-array abstraction such as `GPtrArray` rather than open-coding pointer-count allocation and growth. The container makes the element shape and resizing contract explicit.

**Confidence:** Extremely high. Direct Guy Harris review on a merged MR, independently identified by Coverity/John Thacker, with the suggested container direction implemented in the merged follow-up !16407.

## Treat allocation attributes as optimizer contracts, not descriptive labels

Compiler allocation attributes can change alias analysis and optimization. They must describe the function's actual return-object semantics, not merely convey that a helper allocates or copies memory.

Merged master MR !15925 removes `G_GNUC_MALLOC` from `wmem_realloc()` and `wmem_memdup()`. Its rationale follows GCC's `malloc`-attribute contract: realloc-like functions can return storage related to an existing object, and a memory-duplication helper can return storage whose contents include pointers to existing objects. Marking those functions as malloc-like can therefore let the compiler make aliasing assumptions that the functions do not guarantee.

**Implementation rule:** apply `G_GNUC_MALLOC` or equivalent compiler attributes only after verifying the exact aliasing and object-reachability contract required by the compiler. Do not annotate realloc-like APIs with a malloc-like attribute, and do not assume a helper qualifies merely because it returns newly allocated memory.

**Review rule:** when changing compiler attributes on memory APIs, treat the change as a correctness/optimization-contract change rather than cosmetic annotation cleanup. Check the compiler's documented semantics and audit callers only after the declaration accurately describes the function.

**Confidence:** Very high. Merged master correction accepted by Anders Broman, with the MR explicitly tied to GCC's documented attribute semantics.
