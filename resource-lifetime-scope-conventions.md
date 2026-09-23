# Wireshark Resource Lifetime-Scope Conventions

This file records durable conventions for matching allocation/resource lifetime to Wireshark's logical reset boundaries. Current upstream source remains authoritative.

## Match the allocator scope to the logical reset boundary, not merely the nearest existing scope

A wmem scope should model how long state is semantically valid. If state is rebuilt on profile changes, new captures, or another explicit reset boundary, placing it in a broader process/epan scope can retain every obsolete generation even when the code correctly drops all references to the old tables.

Merged master MR !11837, authored and merged by John Thacker, fixes address-resolution tables that were recreated by `init_dissection()` but allocated in epan scope. Most entries are logically reset when a profile changes, a new file is loaded, or TShark `-M` starts another dissection pass; epan-scope allocation therefore leaked each discarded generation until process exit. The accepted change introduces a dedicated wmem allocation scope whose lifetime matches that address-resolution reset boundary. It deliberately does not move manually entered GUI resolutions into the new scope because those entries have different persistence semantics, and it cannot simply use file scope because some enterprise-name lookups are needed during dissector registration before file scope is active.

**Implementation rule:** choose allocation scope from the state object's semantic lifetime. If data is reset independently of packet, file, or process lifetime, use or introduce a scope that follows that reset boundary instead of allocating from a broader convenient scope and merely overwriting the owning pointer.

**Review rule:** whenever an init/reset routine rebuilds scoped containers, ask what frees the previous generation. Also verify exceptional consumers that need the state before or after the obvious scope is active; those consumers may justify a distinct lifetime domain rather than forcing the data into an existing packet/file/epan scope.

**Confidence:** Very high. Merged master leak fix authored and merged by John Thacker, with the profile/new-file/TShark reset semantics and pre-file-scope registration dependency documented in the MR.

## Scope-managed containers do not automatically release external-library resources

Wmem can reclaim container nodes and ordinary wmem allocations automatically, but an object that owns a resource from another library still requires that library's release operation. Tie such cleanup to the same lifetime boundary as the state that owns the resource.

Merged master MR !11838, authored and merged by John Thacker, fixes SSH decryption state containing libgcrypt cipher handles retained in file-scope conversation data. Moving the surrounding state into a wmem lifetime is not enough to close `gcry_cipher_hd_t` handles, so the accepted change registers file-scope cleanup callbacks that call `gcry_cipher_close()` for the stored contexts when the scope is destroyed.

**Implementation rule:** distinguish memory reclamation from resource destruction. For file/conversation state that owns handles, references, cloned buffers, descriptors, or other resources with an explicit destructor, register cleanup at the owning scope's teardown (or use an owning wrapper/container with equivalent destructor semantics).

**Review rule:** for every scoped struct/container, inspect payload members for non-wmem ownership contracts. Automatic scope teardown proves only that wmem-managed storage is reclaimed; it does not prove that external resources were released.

**Confidence:** Very high. Merged master resource-leak fix authored and merged by John Thacker, directly exercising file-scope cleanup for libgcrypt contexts.
