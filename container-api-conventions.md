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

## Make container shape match the API's real cardinality

Do not represent an operation that can return exactly one collection as a list containing one collection merely to preserve hypothetical multiplicity that the call contract does not support. Artificial nesting leaks impossible states into callers and multiplies iteration, ownership, and cleanup code.

Merged MR !26355, authored by John Thacker and approved/merged by Gerald Combs, removes a `GList`-of-`GList` shape from extcap configuration retrieval. The relevant callback is invoked for one extcap binary/interface configuration; there is no valid path in which callers need several matching configuration lists. Returning the argument list directly simplifies the Qt consumer, required-argument checks, capture argument construction, and the corresponding destructor.

**Implementation rule:** encode actual cardinality in APIs and data structures. Use an outer collection only when multiple independent inner collections are semantically possible; otherwise return the collection itself and let its ownership contract be explicit.

**Confidence:** Extremely high. Merged master simplification authored by John Thacker and explicitly approved/merged by project lead Gerald Combs.

## Bound archive/container parsing by construction

For attacker-controlled archive-like input, prefer parsing strategies that do not require trusting a distant end-of-file structure and avoid automatic recursive expansion when recursion is not necessary to the feature. This both fits streaming/subdissector contexts and limits amplification attacks.

Merged MR !25738 adds ZIP support to Fileshark using a forward streaming parser rather than scanning backward from the end for the Central Directory, because an enclosing media-type dissector may not provide arbitrary end-of-file access. It also deliberately declines recursive dissection of files contained inside the ZIP as a ZIP-bomb mitigation. During review, Martin Mathieson caught a separate field-width mismatch (`FT_UINT16` displayed with length 4), reinforcing that archive record widths still need ordinary dissector field-contract checks.

**Implementation rule:** where the format permits it, parse untrusted containers incrementally from available framing rather than assuming random EOF access. Do not recursively auto-expand nested containers unless the product requirement justifies the resource-amplification risk and explicit bounds are in place.

**Confidence:** Very high. Merged master implementation by John Thacker with focused maintainer review.

## Use growable containers when parsed cardinality cannot be predicted exactly

Do not preallocate a fixed array from a mathematical estimate when malformed or partial records can make the parser produce more logical entries than that estimate predicts. A floor-divided byte count is only a safe capacity calculation if the parser is guaranteed to reject every incomplete trailing record before appending an entry.

Merged MR !25691, authored and merged by John Thacker, fixes the Z39.50 MARC directory parser. It had sized a raw directory array from `(directory_bytes / entry_size)`, but the loop could still append an entry for a partial directory record, making the allocated capacity one entry too small. The accepted implementation replaces the predicted fixed array with `wmem_array_t`, appends each successfully encountered logical entry, and obtains the final count from the container.

**Implementation rule:** when record count is genuinely data-dependent, append to a growable container and derive cardinality from what was actually parsed. If a fixed allocation is preferable, prove and enforce the exact parser invariant that makes the capacity formula an upper bound before writing any entry.

**Confidence:** Very high. Merged master memory-safety fix authored and merged by John Thacker, with the failure mode and PoC provenance documented in the MR.

## Sort unordered container keys before producing stable external output

Hash-table iteration order is an implementation detail and may be deliberately randomized. If traversal order feeds user-visible diagnostics, serialized output, or regression-test text, impose an explicit ordering rather than letting the container choose it.

Merged MR !25598, authored and merged by John Thacker, adds `wmem_map_get_keys_sorted()` and uses it for DNS resolver-state diagnostics. The motivating bug was nondeterministic output caused by randomized map iteration, which made the same capture produce unstable regression-test results.

**Implementation rule:** unordered containers are appropriate for lookup, but not as an ordering contract. Before emitting order-sensitive external results, materialize the relevant keys/items and sort them with a comparator that reflects the semantic output order.

**Confidence:** Very high. Merged master API and deterministic-output fix authored and merged by John Thacker.