# Wireshark Allocator-Scope Conventions

This file records durable allocator-family and lifetime-scope rules extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Match allocation/free families and prefer scope-managed lifetime when it fits

An object allocated from a wmem scope must not be released with a GLib allocator routine. Allocation family and scope are part of the ownership contract, not interchangeable implementation details.

Merged MR !17596 (`RELOAD Framing: Make sure we have valid addresses`) changed a temporary transaction-key buffer from `g_malloc()` to `wmem_alloc(wmem_file_scope())`. Merged follow-up MR !17606 fixes the resulting allocator mismatch: the code still called `g_free()` and was changed to `wmem_free(wmem_file_scope(), ...)`.

During !17606 review, Gerald Combs suggested an even narrower ownership model: allocate the key using `pinfo->pool` and do not explicitly free it. John Thacker agreed. That review direction is consistent with Wireshark's broader scope-managed lifetime model: if data only needs to survive the current packet dissection, packet scope is preferable to a broader file scope plus manual cleanup.

**Implementation rule:** pair wmem allocation/free calls with the same allocator scope and never cross allocator families (`wmem_*` versus `g_*`). Choose the narrowest scope that satisfies the required lifetime. When packet lifetime is sufficient, prefer `pinfo->pool`/packet scope and let scope teardown perform cleanup rather than introducing a manual free.

**Review rule:** when changing an allocation API or allocator scope, audit the entire ownership path—including cleanup, containers, exceptional exits, and redissection lifetime—not just the allocation line.

**Confidence:** Very high. Merged correctness fix plus direct review from Gerald Combs and agreement from John Thacker; the same lifetime principle is independently represented elsewhere in the notebook.

## Pass short-lived borrowed data explicitly instead of hiding it in broader context state

A pointer can be technically valid for the duration of a call and still be a poor member of a longer-lived context structure. If a value is produced for one immediate operation and its lifetime does not match the context object, make that borrowed lifetime visible in the function signature.

Merged MR !25579, authored and merged by Guy Harris, repaired an IEEE 802.15.4 decryption crash by temporarily pointing `decrypt_info->key` at a stack key whose use was confined to the called decryption routines. Guy's later merged MR !25593 improved the interface by removing that key pointer from the broader context and passing the key directly to the decryption helper.

**Implementation rule:** do not stash short-lived borrowed pointers in a context struct merely because all current callees happen to use them synchronously. Prefer an explicit parameter when the data belongs to one operation; reserve context members for values whose ownership and lifetime genuinely match the context.

**Confidence:** Extremely high. Two merged master changes authored by Guy Harris, with the later change representing the preferred evolved interface.