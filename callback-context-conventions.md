# Wireshark Callback Context Conventions

This file records durable conventions for C callback APIs whose function pointers are paired with callback-specific context/data pointers. Current upstream source and API documentation remain authoritative.

## Treat a callback and its context pointer as one semantic pair

When an API stores several callbacks and gives each callback its own `void *` context value, the callback function and that context pointer form one logical contract. Context values are not interchangeable merely because their C types are identical or because neighboring callback records happen to contain similar data.

Merged master MR !13652 fixes UAT file loading after the loader invoked a field's `set` callback with `cbdata.chk` rather than the `set` callback's own `cbdata.set`. The bug could corrupt memory when a plugin supplied distinct `set_data`; the Qt UAT path already paired the callback with the correct context, making the mismatch in the file-loading path clear.

**Implementation rule:** whenever a callback table contains callback-specific user data, pass the context stored for that exact callback. Treat `(function, context)` as a unit when copying, storing, invoking, or refactoring callback registrations.

**Review rule:** callback families such as `check`, `set`, `free`, compare, hash, or notification handlers deserve a paired audit: verify both the function selected and the matching context slot at every invocation. A compiler-clean `void *` argument provides no protection against selecting the wrong semantic context.

**Testing rule:** exercise callback users that deliberately provide different context objects for neighboring callbacks. This exposes accidental cross-wiring that can remain invisible when all callback context pointers are NULL or happen to reference the same object.

**Confidence:** Very high. Merged master memory-correctness fix with a concrete callback/context mismatch and an existing correct parallel call path for comparison.

## Carry negotiated runtime metadata explicitly through plugin callback context

A plugin's identity does not necessarily determine all of the parameters needed to interpret its data. When behavior depends on values negotiated elsewhere in the protocol stack, pass those values explicitly through a typed callback context rather than requiring the plugin to infer them from global state, payload numbers, or unrelated caller internals.

Merged master MR !11378, authored and merged by John Thacker, changed the codec callback interface from an opaque codec-private `void *` to a `codec_context_t` that carries the RTP/SDP-negotiated sample rate and channel count alongside a separate `priv` member for decoder-owned state. That lets dynamically assigned RTP payload formats such as L16 use parameters supplied by SDP while preserving codec-specific state behind the same callback interface. Follow-up MR !11410 extends the same direction by passing negotiated `fmtp` data so AMR can distinguish octet-aligned from bandwidth-efficient framing.

**Implementation rule:** if a callback implementation needs both caller-owned negotiated/configuration metadata and plugin-owned mutable state, model those as distinct members of an explicit context object. The caller should initialize the shared metadata; the plugin should own only its private state portion.

**Architecture rule:** avoid baking negotiated parameters into plugin selection or assuming that a registered codec/protocol name uniquely determines wire behavior. Dynamic protocol assignments should make their negotiated context an explicit part of the API contract.

**Review rule:** when widening a plugin callback API, check creation, decode/use, query, and teardown paths together. A typed wrapper context is useful only if every callback receives the same semantic context and private-state ownership remains unambiguous.

**Confidence:** Very high. Merged master codec-interface redesign authored and merged by John Thacker, immediately followed by additional negotiated-parameter use in merged !11410.
