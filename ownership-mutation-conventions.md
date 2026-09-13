# Wireshark Ownership and Mutation Conventions

This file records durable conventions for mutation ordering and container-owned object lifetime. Current upstream source remains authoritative.

## Unlink objects before destroying them when the container operation may inspect the object

If a container removal operation compares, hashes, traverses, or otherwise refers to an element pointer, remove the element while the pointed-to object is still alive. Freeing the object first and then asking the container to remove that pointer can create a use-after-free even when the caller no longer intends to read the object itself.

Merged MR !25029 fixes a funnel-menu use-after-free by calling `g_slist_remove()` before `free_funnel_menu()`. The old order freed the menu and then passed the freed object through list removal. Merged MR !25042 provides complementary ownership guidance in ETW state: it replaces manual free/remove/insert sequences with a `GHashTable` value-destroy callback plus `g_hash_table_replace()`, expressing replacement ownership directly in the container.

**Implementation rule:** perform structural unlink/removal before object destruction whenever the container still needs the element identity, and prefer container-native destroy/replacement hooks when they can encode ownership centrally. Avoid manual remove/free/insert sequences whose correctness depends on subtle ordering duplicated at every call site.

**Confidence:** Very high. Two merged master correctness changes, including a direct use-after-free fix and a container-ownership cleanup.

## Keep ownership bookkeeping separate from mutable pointer views

When an API is permitted to rewrite pointer slots supplied by its caller, the array passed into that API is not a reliable ownership ledger after the call returns. The owner must retain an independent list or container containing the allocations that must eventually be released.

Merged master MR !21101 fixes `cli_main` after noting that ISO C permits user code to modify the strings and pointer entries in `argv`. The wrapper allocates a UTF-8 argument vector, but `real_main()` may legally change that vector. The accepted fix stores the allocated strings in a private `GPtrArray` while passing `real_main()` a separate pointer array; cleanup then walks the private ownership container rather than potentially modified `argv` entries. The same fix was accepted on maintained branches in !21122, !21123, and !21124.

**Implementation rule:** distinguish an owning collection from a mutable API view. If the callee contract allows pointer substitution, reordering, clearing, or other slot mutation, do not use that view later to decide which allocations to free. Retain independent ownership references whose identity is not exposed to the mutating callee.

**Review implication:** when a caller allocates objects and passes an array of their pointers to another API, check whether the callee may mutate the array itself as well as the pointed-to objects. Cleanup through the post-call array is safe only when the API contract guarantees those pointer identities remain intact.

**Confidence:** Very high. Merged master correctness fix plus three stable backports; the motivating mutability guarantee is part of the C `argv` contract rather than an incidental implementation detail.
