# Wireshark Library-Layering Conventions

This file records durable dependency-direction conventions for reusable Wireshark libraries. Current upstream source remains authoritative.

## Put shared low-level capabilities in the lowest common owning layer

When two higher-level libraries need the same generic capability, do not make one higher-level sibling depend on the other merely to reuse its implementation. Move the capability to the lowest common layer that semantically owns it, then have both higher layers depend downward on that implementation.

Merged master MR !21899, authored and merged by Guy Harris, moves compressed-file writing from Wiretap to `libwsutil`. The stated goal is to remove `libwritecap`'s dependency on `libwiretap`—and the hacks required to make that dependency work—while also allowing Wiretap to use the same routines rather than keeping a duplicate compressed-writing implementation. Compression I/O is a generic utility capability, so `wsutil` is the common lower layer that can be consumed by both libraries without reversing the dependency graph.

**Architecture rule:** choose shared-code placement by semantic ownership and dependency direction, not by where the first implementation happened to live. If reuse would make a lower/sibling library depend upward or sideways on a more specialized subsystem, extract the genuinely generic capability downward and converge duplicate implementations on it.

**Review check:** when moving a helper for reuse, inspect the resulting link graph as well as the source diff. A successful refactor should eliminate special linkage hacks and duplicate implementations rather than merely relocating one copy while preserving an architectural cycle or sibling dependency.

Merged !21906, !21894, and !21896 independently reinforce the same broader principle at extension boundaries: generic Decode As, UI, and Lua infrastructure should use generic callbacks/adapters or isolate protocol-specific helpers instead of acquiring direct protocol dependencies. Those points are already covered in `application-layer-boundary-conventions.md`, so they are corroborating evidence rather than duplicate rules here.

**Confidence:** Very high. The primary exemplar is a merged master architectural refactor authored and merged by Guy Harris with the dependency objective stated directly.

## Keep low-level executables independent of UI libraries

A command-line or privileged low-level executable should not acquire a dependency on the UI library merely because a few generic helpers were first implemented there. Put command-line parsing, generic filter-file handling, and other non-UI utilities in the lowest reusable layer that owns them.

Merged master MR !9780 moves `cmdarg_err`, common command-line option helpers, and filter-file handling from `ui/` to `wsutil/`, exports the needed interfaces, updates their consumers, and removes `dumpcap`'s link dependency on `ui`. The implementation is broad because many CLI, GUI, extcap, and fuzzing consumers already depended on those helpers; the architectural direction is nonetheless simple: `dumpcap` should depend on generic utility code, not on an application-presentation layer.

**Architecture rule:** if a low-level executable needs functionality currently owned by a higher-level UI/application library, first determine whether the functionality is actually UI-specific. If it is generic, move the capability downward to the common owning layer and update all consumers rather than preserving an inverted dependency for convenience.

**Confidence:** High. Merged master architectural refactor that directly removes the unwanted link dependency.
