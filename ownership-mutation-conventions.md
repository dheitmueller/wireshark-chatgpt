# Wireshark Ownership and Mutation Conventions

This file records durable conventions for mutation ordering and container-owned object lifetime. Current upstream source remains authoritative.

## Unlink objects before destroying them when the container operation may inspect the object

If a container removal operation compares, hashes, traverses, or otherwise refers to an element pointer, remove the element while the pointed-to object is still alive. Freeing the object first and then asking the container to remove that pointer can create a use-after-free even when the caller no longer intends to read the object itself.

Merged MR !25029 fixes a funnel-menu use-after-free by calling `g_slist_remove()` before `free_funnel_menu()`. The old order freed the menu and then passed the freed object through list removal. Merged MR !25042 provides complementary ownership guidance in ETW state: it replaces manual free/remove/insert sequences with a `GHashTable` value-destroy callback plus `g_hash_table_replace()`, expressing replacement ownership directly in the container.

**Implementation rule:** perform structural unlink/removal before object destruction whenever the container still needs the element identity, and prefer container-native destroy/replacement hooks when they can encode ownership centrally. Avoid manual remove/free/insert sequences whose correctness depends on subtle ordering duplicated at every call site.

**Confidence:** Very high. Two merged master correctness changes, including a direct use-after-free fix and a container-ownership cleanup.