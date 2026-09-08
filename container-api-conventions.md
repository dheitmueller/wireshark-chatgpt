# Wireshark Container and Key Representation Conventions

This file records durable conventions for choosing container APIs, key representations, and allocator domains. Current upstream source remains authoritative.

## Hash and equality functions must match the key's actual representation and alignment

Do not select a scalar hash merely because a key is the same width as that scalar. Hash helpers such as GLib's integer hash may dereference the supplied pointer as an integer and therefore impose alignment and object-representation requirements that raw packet bytes do not satisfy.

Merged MR !26327, authored and merged by John Thacker, fixes WireGuard state lookup where keys came directly from packet-derived byte storage but were passed to `g_int_hash()`. Those byte addresses are not guaranteed to be integer-aligned. The accepted implementation uses a byte-oriented hash instead.

**Implementation rule:** choose hash/equality routines from the storage contract of the key, not only its logical value. Raw or packed protocol bytes require byte-safe operations unless they have first been copied into a properly aligned typed object.

**Confidence:** Very high. Merged master portability/correctness fix authored and merged by John Thacker.

## Match the container allocator to the lifetime of the object that owns it

A container embedded in file- or conversation-scope state should normally use the same lifetime domain as its owner. Mixing a long-lived wmem owner with a separately allocated GLib container creates a second teardown obligation and is especially error-prone when the owning state is intentionally reclaimed as a scope.

Merged MR !26328, authored and merged by John Thacker, changes the TWAMP session list stored in `wmem_file_scope()` conversation state from `GSList` to `wmem_list_t`. The old list nodes were never freed independently; using the wmem container makes their lifetime match the state that owns them.

**Implementation rule:** when collection nodes have exactly the same lifetime as their enclosing wmem state, allocate the collection from that wmem scope. Use independently managed containers only when they genuinely need an independent lifetime or mutation/ownership contract.

**Confidence:** Very high. Merged master lifetime fix authored and merged by John Thacker.

## Prefer direct lookup or iteration over materializing temporary key/value lists

If code only needs to test one key or walk a hash table, do not allocate a snapshot list of keys or values unless snapshot semantics are required. Direct lookup and `GHashTableIter` avoid temporary ownership, cleanup obligations, and needless allocations.

Merged MR !26330 removes an extcap key-list snapshot in favor of direct hash lookup while simplifying error-path ownership. Merged MR !26336 extends the same approach by replacing additional `g_hash_table_get_keys()` / `g_hash_table_get_values()` traversals with `GHashTableIter`. Both were authored and merged by John Thacker.

**Implementation rule:** use the narrowest table operation that expresses the task: lookup for membership/value retrieval, iterator for traversal, and an allocated key/value list only when a stable independent snapshot is semantically necessary.

**Confidence:** Very high. Two merged master extcap cleanups authored and merged by John Thacker.

## After transferring ownership from a container element, neutralize that slot and use the canonical destructor

Special cleanup functions that know which members were 'stolen' tend to diverge from the normal destructor. When a container supports it, transfer ownership by extracting the object and setting the source slot to `NULL`, then run the ordinary cleanup path if that destructor is explicitly NULL-safe.

Merged MR !26330 simplifies extcap error handling this way: after stealing an `extcap_interface`, the list element is cleared and the normal freeing routine is used rather than a second partial-free function.

**Implementation rule:** prefer one canonical destructor plus an explicit ownership-transfer operation over parallel 'free everything except the stolen member' routines. This is appropriate only when the canonical destructor's NULL behavior is part of the API contract.

**Confidence:** Very high. Merged master ownership cleanup authored and merged by John Thacker.