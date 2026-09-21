# Wireshark Semantic Context Parameter Conventions

This file records durable conventions for threading packet and allocator context through helper APIs. Current upstream source remains authoritative.

## Pass the narrowest context object that matches the helper's responsibility

Avoid ambient access to packet-lifetime state when the relevant lifetime context can be expressed directly in the call graph. At the same time, do not mechanically reduce every helper to a raw allocator parameter if the helper is semantically packet-oriented and naturally needs packet metadata.

Merged master MR !14990, approved and merged by Anders Broman, replaces a first set of `wmem_packet_scope()` calls with explicit context. The MR's stated rule is to reuse an existing `packet_info *` when the caller already has one; give narrowly memory-focused helpers a `wmem_allocator_t *`; and give packet-oriented helpers a `packet_info *` and obtain packet-lifetime storage from `pinfo->pool`. The change intentionally leaves harder cases for later rather than obscuring API responsibility with a sweeping mechanical rewrite.

**Implementation rule:** make lifetime dependencies visible in function parameters. Use `wmem_allocator_t *` when allocation itself is the helper's primary abstraction; use `packet_info *` when the helper operates on packet semantics and allocation is only one facet of that context. Prefer `pinfo->pool` over reaching for global/ambient packet scope when `pinfo` is already available.

**Refactoring rule:** when removing ambient context, refactor incrementally and choose parameter types according to semantic responsibility rather than minimizing parameter size or mechanically applying one signature everywhere.

**Confidence:** High. Merged master refactoring with explicit design criteria in the MR description and maintainer acceptance by Anders Broman.
