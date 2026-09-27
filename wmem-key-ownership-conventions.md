# wmem Key Ownership Conventions

Merged MRs !7018 and !7028 fix conversation keys whose backing storage did not live as long as the map or conversation retaining them. !7028 states the key property directly: `wmem_map` does not copy keys. The accepted code duplicates complete typed keys into file scope.

Merged !7047 removes an ordinary `g_free()` of memory owned by a wmem scope.

**Rule:** a retained container key must have storage whose lifetime covers the entry. Determine whether the container copies or retains key pointers. For wmem-owned storage, release it through the owning allocator scope rather than mixing allocator families.

**Confidence:** Very high. Three merged core fixes from Gerald Combs.
