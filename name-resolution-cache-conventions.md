# Wireshark Name-Resolution Cache Conventions

This file records durable conventions for caching and presenting resolved protocol/address names. Current upstream source remains authoritative.

## Never cache a more-specific namespace entry under a less-specific key

A cache key must preserve enough identity to distinguish every semantic value that can be stored under it. If a resolver can return entries at different prefix lengths, inserting a longer-prefix result into a shorter-prefix cache can make that result falsely apply to unrelated values that merely share the shorter prefix.

Merged master MR !13512, authored and merged by John Thacker, fixes manufacturer resolution for IEEE MA-L/MA-M/MA-S assignments. Wireshark's `manuf_hashtable` is keyed by a 24-bit OUI and its stored `hashmanuf_t` representation only accommodates that 24-bit namespace. The general manufacturer lookup can also return 28-bit and 36-bit MA-M/MA-S matches. Caching one of those results in the 24-bit table would therefore make other addresses with the same first 24 bits incorrectly resolve to the longer-prefix vendor. The accepted code restricts the 24-bit cache path to `ws_manuf_lookup_oui24()` and handles the more-specific global lookups separately when resolving complete Ethernet addresses.

**Implementation rule:** before caching a resolver result, compare the specificity of the returned identity with the specificity represented by the cache key and stored value type. Only cache it there if the representation can preserve the complete identity. Otherwise use a more-specific cache/key type or leave the result in the authoritative lookup structure.

**Review rule:** for prefix, mask, range, wildcard, and hierarchical name-resolution tables, test two inputs that share the coarse cache key but differ outside it. A lookup of the first input must not influence the second unless the authoritative namespace says they have the same resolution.

**Architecture rule:** cache shape should follow the resolver's semantic namespace, not be stretched merely to reuse an existing fast path. If an old cache only represents one assignment class, adding new assignment classes may require separate lookup/storage rather than silently projecting them into the old key space.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker, with the cache-width limitation and incorrect-match risk documented directly in the code comments and accepted diff.