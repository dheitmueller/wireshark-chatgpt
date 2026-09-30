# Wireshark Library-Layering Conventions

This file records durable dependency-direction conventions for reusable Wireshark libraries. Current upstream source remains authoritative.

## Put shared low-level capabilities in the lowest common owning layer

When two higher-level libraries need the same generic capability, do not make one higher-level sibling depend on the other merely to reuse its implementation. Move the capability to the lowest common layer that semantically owns it, then have both higher layers depend downward on that implementation.

Merged master MR !21899, authored and merged by Guy Harris, moves compressed-file writing from Wiretap to `libwsutil`. The stated goal is to remove `libwritecap`'s dependency on `libwiretap`—and the hacks required to make that dependency work—while also allowing Wiretap to use the same routines rather than keeping a duplicate compressed-writing implementation. Compression I/O is a generic utility capability, so `wsutil` is the common lower layer that can be consumed by both libraries without reversing the dependency graph.

**Architecture rule:** choose shared-code placement by semantic ownership and dependency direction, not by where the first implementation happened to live. If reuse would make a lower/sibling library depend upward or sideways on a more specialized subsystem, extract the genuinely generic capability downward and converge duplicate implementations on it.

**Review check:** when moving a helper for reuse, inspect the resulting link graph as well as the source diff. A successful refactor should eliminate special linkage hacks and duplicate implementations rather than merely relocating one copy while preserving an architectural cycle or sibling dependency.

Merged !21906, !21894, and !21896 independently reinforce the same broader principle at extension boundaries: generic Decode As, UI, and Lua infrastructure should use generic callbacks/adapters or isolate protocol-specific helpers instead of acquiring direct protocol dependencies. Those points are already covered in `application-layer-boundary-conventions.md`, so they are corroborating evidence rather than duplicate rules here.

**Confidence:** Very high. The primary exemplar is a merged master architectural refactor authored and merged by Guy Harris with the dependency objective stated directly.

## Generic numeric and address formatting belongs below EPAN

A helper that performs generic numeric or address-to-text conversion does not inherently belong in the packet-analysis layer merely because its first users were there. If the operation has no EPAN-specific state or semantics, locating it in `wsutil` lets lower-level and higher-level consumers share the implementation without acquiring an EPAN dependency.

Merged master MR !4212, authored by João Valverde, moves several numeric and IP formatting routines and their exported symbols from `epan/to_str` to `wsutil/to_str`, adjusts callers, and updates the shared-library symbol lists accordingly.

**Architecture rule:** place generic conversion and formatting primitives in the lowest library that semantically owns them. Moving an exported helper across libraries is an ABI/package-surface change as well as a source refactor, so update symbol manifests and callers together.

**Confidence:** Very high. Merged master library-layering refactor by João Valverde. It independently corroborates the later, stronger Guy Harris !21899 rule above about moving genuinely generic capabilities into the lowest common owning layer.

