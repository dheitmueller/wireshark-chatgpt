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

## Prefer scope-managed ownership across exception-capable dissection paths

Manual cleanup at the end of a dissector is not exception-safe when bounds checks or nested dissection can throw before the cleanup point. For temporary data whose lifetime naturally matches a Wireshark allocator scope, use scope-managed allocation so exceptional exits do not leak it.

Merged master MR !24823, authored and merged by John Thacker after an OSS-Fuzz report, replaces `g_strsplit()` with `wmem_strsplit()` in ISAKMP parsing. The GLib allocation could leak whenever a later TVB operation raised an exception before the explicit free; tying the split result to wmem lifetime removes that exceptional cleanup obligation.

**Implementation rule:** when allocating temporary data in dissectors, audit every operation between allocation and manual free for exception-capable TVB access or nested dissection. Prefer `wmem_*` ownership at the narrowest suitable scope when it makes cleanup automatic. If manual ownership is unavoidable, the cleanup mechanism must remain correct on exceptional exits rather than only on the normal return path.

**Confidence:** Very high. Merged master leak fix authored and merged by John Thacker with a concrete OSS-Fuzz trigger.

## Mark allocation results as must-use when ignoring them would lose ownership or correctness

Allocation APIs whose return value is the only usable reference to the new allocation should make accidental result-dropping visible to the compiler. This is especially important for reallocation, where a successful call may invalidate the old pointer and return a different address.

Merged MR !24646, authored by John Thacker and merged by Michael Mann, adds `WS_WARN_UNUSED` to `wmem_alloc()`, `wmem_alloc0()`, and `wmem_realloc()`. The MR notes that current code did not intentionally ignore these results; the annotation exists to catch future typos, and the reallocation result in particular must always be consumed.

**Implementation rule:** apply must-use/warn-unused annotations to ownership-producing APIs when discarding the returned object is almost certainly a bug. For realloc-like APIs, always assign/check the returned pointer according to the API's failure contract before assuming the original pointer remains usable.

**Confidence:** Very high. Merged public wmem API hardening authored by John Thacker and accepted by a senior maintainer.

## A second long-lived owner of a reference-counted object requires its own reference

Adding a reference-counted object to another owner or container does not make two owners safe unless the reference count represents both ownership claims. Error unwinding can expose this immediately: one path may release the transient/local reference while later container teardown releases the same object again.

Merged release-4.6 MR !24613 fixes a pcapng Darwin PIB use-after-free/double-unref path. `pcapng_read_darwin_legacy_block()` created a block and added it to `wth->dpibs`, leaving both the current block path and the long-lived array able to release it. If the caller subsequently failed for an unrelated reason, open-error cleanup unreferenced the block and later `wtap_block_array_free(wth->dpibs)` unreferenced it again. The accepted fix takes an additional `wtap_block_ref()` when the object is added to the second owner.

**Implementation rule:** audit every ownership edge when a reference-counted object is inserted into persistent state. If both the existing path and the new container can independently outlive/release the object, acquire a reference for the new owner at insertion time; do not rely on the success path to hide an under-counted ownership graph.

**Confidence:** Very high. Merged memory-safety backport by John Thacker with the two independent release paths explicitly documented.

## Container migrations must preserve key/value ownership semantics

Replacing one container with another can silently change whether keys or values are copied or merely referenced. A caller pattern that was safe with a copying container can become a use-after-return if the replacement stores the caller's pointer.

Merged master MR !23646, authored by John Thacker and accepted by Gerald Combs after OSS-Fuzz found a stack-use-after-return, fixes exactly this during a GUID lookup migration. The old `wmem_tree_insert32_array()` path copied the GUID key, so callers could pass an `e_guid_t` on the stack. The newer `wmem_map` path retained the key pointer; the accepted fix explicitly copies the GUID into `wmem_epan_scope()` before insertion.

**Implementation rule:** when changing a container type or insertion API, audit its copy/borrow/ownership contract for both keys and values. If the container retains a pointer, copy borrowed or stack-backed data into storage whose lifetime covers the container; do not assume the old container's copy semantics carry across the refactor.

**Review rule:** treat data-structure migrations as ownership/lifetime changes even when the visible lookup API is unchanged. Fuzz-detected lifetime failures are a strong signal to inspect every analogous insertion site.

**Confidence:** Very high. Merged master memory-safety fix authored by John Thacker, with a concrete OSS-Fuzz stack-use-after-return trigger and senior-maintainer acceptance.
