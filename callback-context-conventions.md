# Wireshark Callback Context Conventions

This file records durable conventions for C callback APIs whose function pointers are paired with callback-specific context/data pointers. Current upstream source and API documentation remain authoritative.

## Treat a callback and its context pointer as one semantic pair

When an API stores several callbacks and gives each callback its own `void *` context value, the callback function and that context pointer form one logical contract. Context values are not interchangeable merely because their C types are identical or because neighboring callback records happen to contain similar data.

Merged master MR !13652 fixes UAT file loading after the loader invoked a field's `set` callback with `cbdata.chk` rather than the `set` callback's own `cbdata.set`. The bug could corrupt memory when a plugin supplied distinct `set_data`; the Qt UAT path already paired the callback with the correct context, making the mismatch in the file-loading path clear.

**Implementation rule:** whenever a callback table contains callback-specific user data, pass the context stored for that exact callback. Treat `(function, context)` as a unit when copying, storing, invoking, or refactoring callback registrations.

**Review rule:** callback families such as `check`, `set`, `free`, compare, hash, or notification handlers deserve a paired audit: verify both the function selected and the matching context slot at every invocation. A compiler-clean `void *` argument provides no protection against selecting the wrong semantic context.

**Testing rule:** exercise callback users that deliberately provide different context objects for neighboring callbacks. This exposes accidental cross-wiring that can remain invisible when all callback context pointers are NULL or happen to reference the same object.

**Confidence:** Very high. Merged master memory-correctness fix with a concrete callback/context mismatch and an existing correct parallel call path for comparison.
