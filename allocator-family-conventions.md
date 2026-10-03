# Allocator-Family Conventions

## Scope-owned wmem allocations are not individually freed with GLib

Merged master MR !96 fixes multipart parsing after a `g_free(start_boundary)` call was left behind when the string began being allocated through `wmem_packet_scope()`. Stable backports !98, !99, and !100 carry the same correction.

**Implementation rule:** allocator family and lifetime scope define ownership. Packet/file-scope wmem allocations are reclaimed by their scope; do not pass them to `g_free()` just because a local error path is done with the pointer.

**Review rule:** after changing a helper to allocate from wmem, audit every early return and error path for stale manual frees from the prior ownership model.

**Evidence weight:** Very high. Merged master memory-safety fix with three maintained-branch backports.
