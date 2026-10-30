# Wireshark Address-Copy and Lifetime Conventions

This file records durable conventions for copying Wireshark `address` values when the copy's lifetime differs from the source. Current upstream source and API documentation remain authoritative.

## Choose address-copy semantics from the destination lifetime

Copying an `address` is an ownership decision, not just a struct assignment. A temporary lookup key and persisted conversation/file state have different lifetime requirements even when they contain the same address at the instant of the copy.

Merged master MR !13551, authored and merged by John Thacker, fixes RDP address handling by making that distinction explicit. For stack-local structs used only as temporary lookup keys, the accepted change uses `copy_address_shallow()`. For address members retained in conversation data allocated from `wmem_file_scope()`, it uses `copy_address_wmem(wmem_file_scope(), ...)`. The MR was specifically motivated by the generic `copy_address()` usage being considered leaky in those paths.

**Implementation rule:** select the address-copy helper according to the lifetime and ownership of the destination. Use a shallow/borrowed copy only when all referenced address storage is guaranteed to outlive the temporary use. When the destination persists in conversation, file, or another allocator-scoped state object, copy the address data into that same suitable scope rather than retaining a borrowed pointer or creating ownership that is never released.

**Review rule:** for each stored `address`, identify (1) how long the destination survives, (2) whether the copy is shallow or owns duplicated storage, and (3) which allocator owns that storage. Do not review `address` copies as ordinary value copies merely because the outer structure itself is copied by value.

**Confidence:** Very high. Merged master correctness/lifetime cleanup authored and merged by John Thacker, with the temporary-versus-file-scope distinction stated explicitly in the MR description.

## RTP tap lookup keys illustrate the shallow-versus-owned distinction

Merged master MR !9921, authored by John Thacker, separates `rtpstream_id_copy_pinfo_shallow()` from the deep-copy helper. A stack-local RTP stream ID used only to search the current tap's collection borrows packet addresses; an ID retained in a persistent list must own copies. The new helper's header explicitly warns **not** to call `rtpstream_id_free()` on the shallow-copy result, because that operation frees address storage the temporary key does not own.

**Implementation rule:** API names and documentation should make borrowing versus owning semantics explicit, particularly when the same logical key participates in both short-lived lookups and long-lived collections. Never run an owning destructor on a borrowed/shallow key; deep-copy only when the key is retained.

**Confidence:** Very high. Merged master leak fix by John Thacker, independently reinforcing the destination-lifetime rule from !13551.
