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
