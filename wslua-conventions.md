# Wireshark WSLua Conventions

This file records durable WSLua/Lua implementation conventions extracted from upstream Wireshark merge-request review. Current upstream source remains authoritative.

## Use contiguous integer keys when Lua sequence semantics are required

Lua reference-table allocation and Lua sequence semantics are different contracts. `luaL_ref()` is appropriate for opaque registry/reference management, but code that relies on table length (`#`) or ordered append/clear behavior should maintain an actual contiguous integer-key sequence instead of depending on implementation details of the reference freelist.

Merged MR !24348, authored and merged by John Thacker, changes WSLua test-library bookkeeping for Lua 5.5 compatibility. The old code used `luaL_ref()` to populate a table and later treated that table as a sequence; changes in Lua's reference freelist behavior made that assumption unsafe. The accepted code explicitly appends entries at consecutive integer indices because the usage is append-only followed by clear-all.

**Implementation rule:** decide whether a Lua table is an opaque reference store or a sequence. If Wireshark code relies on length/order/contiguity, maintain consecutive integer keys directly; do not infer sequence properties from `luaL_ref()` allocation behavior.

**Confidence:** Very high. Merged compatibility fix authored and merged by John Thacker, with the semantic mismatch identified explicitly in the change rationale.


## Generate Lua constants from shared introspection metadata, not header scraping

When a scripting API exposes constants that are already defined by Wireshark's C API, prefer a single machine-readable/introspection source of truth over a separate script that scrapes headers and reconstructs the same namespace.

Merged MR !8362, authored and merged by João Valverde, moves WSLua constant generation to Wireshark's introspection API and generated enum metadata. The change removes the older Python logic that parsed several C headers to synthesize `init.lua` constants, while preserving the Lua namespace structure used for expert groups/severities and other constant families.

**Architecture rule:** if C enums/constants must be exported to Lua (or another binding), generate or expose them from the same introspection metadata used by the native code. Avoid a second parser over headers when that duplicates semantic knowledge and can drift.

**Namespace rule:** C's flat macro/enum namespace is not a reason to pollute Lua's global namespace; keep related constants in structured Lua tables.

**Confidence:** Very high. Merged master architectural cleanup authored and merged by João Valverde.

## Generic add-and-return APIs should return consistent typed values and offsets

Merged master MR !6343 extends `TreeItem:add_packet_field` so supported field types consistently return the child item, the decoded typed value, and the next offset. The implementation reuses the corresponding `proto_tree_add_item_ret_*` primitives where available, adds missing typed helpers in core code, documents the return tuple, and adds Lua tests that compare the returned values with direct `TvbRange` decoding.

Roland Knall explicitly raised compatibility concerns about changing an established scripting method, and the discussion considered the change in the context of the upcoming major release.

**API rule:** generic scripting bindings should make return behavior consistent across supported field types, document all returned values, and test them against the native decoding primitive. Changes to established binding behavior are compatibility-sensitive even when they make the API more regular.

**Confidence:** High. Merged master API expansion with substantive maintainer review and dedicated tests.


## Provide exact-width paths when Lua numbers cannot represent the native integer domain

Merged !3686 extends ProtoField masks from 32 to 64 bits. Review explicitly identifies the representation mismatch: ordinary Lua numbers are floating-point and cannot exactly represent every 64-bit integer. The accepted binding therefore allows masks to arrive as an ordinary number where appropriate, a decimal string, or Wireshark's exact-width `UInt64` userdata, and adds tests across those argument forms.

**API rule:** do not require a scripting language's default numeric type to carry native integer values outside its exact domain. Expose an exact-width object or other lossless representation path and perform conversion in the native binding.

**Testing rule:** cover the supported argument representations, nil/default handling, invalid values, and platform compiler diagnostics around width/format conversion.

**Confidence:** High. Merged scripting API extension whose design was shaped by explicit discussion of Lua's numeric precision limits.

## Distinguish reported length from captured length

Merged master MR !3353, authored by Guy Harris, makes the WSLua TVBuff length contract explicit: reported length is the logical length the packet or subset had on the network, while captured length is the amount the capture process saved. It adds `Tvb:captured_len()` and retains the established `Tvb:len()` behavior as a backwards-compatible captured-length alias rather than silently changing it. Stable backports !3354 and !3355 preserve the same distinction. Guy's adjacent !3356-!3358 correct `reported_length_remaining()` documentation to match the implementation's zero result rather than an obsolete `-1` description.

**API rule:** expose reported and captured length as separate concepts. Do not call captured length the "actual" length, and preserve established scripting behavior when introducing a more precise API name.

**Dissector rule:** use reported length for the logical on-wire extent unless code specifically needs the amount physically captured.

**Confidence:** Extremely high. Master and stable changes were authored by Guy Harris.

