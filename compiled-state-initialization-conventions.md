# Compiled-state initialization conventions

This file records durable conventions for helpers that compile or construct caller-supplied reusable state. Current upstream implementation remains authoritative.

## A constructor or compiler must fully initialize the destination it owns

When a helper compiles a search pattern, lookup table, bitmap, or similar reusable object into caller-supplied storage, the helper owns the complete post-call representation. Do not rely on the caller having obtained zero-filled storage, used a static-duration object, or cleared a previous compiled value first.

Merged master MR !12362, authored and merged by John Thacker, fixes `ws_mempbrk_compile()` by clearing the complete 256-byte pattern table before setting entries for the requested bytes. Most existing callers happened to use static pattern objects and therefore received zero initialization automatically, which hid the contract bug. A stack-local pattern could begin with arbitrary bits, and recompiling an existing pattern could otherwise leave bits from the earlier compilation set. The same change was accepted on the maintained release branches in !12363, !12368, and !12369.

**Implementation rule:** a function whose job is to construct or compile an object should initialize every byte or logical member that contributes to later behavior before populating the requested state. Initialization belongs with the constructor/compiler unless the API explicitly documents an incremental-update contract.

**Review rule:** when a compiled object is normally static or zero-initialized at declaration, check whether the API would still be correct for stack-local storage and for a second compilation into the same object. Accidental reliance on storage-duration initialization is an API contract smell.

**Testing rule:** for reusable compiled state, include a recompile case where the second input removes state set by the first. Where practical, also exercise non-static caller storage so tests do not inherit zero initialization from the language runtime.

**Confidence:** Very high. Merged master fix authored and merged by John Thacker, with three accepted release-branch backports exposing the same invariant.