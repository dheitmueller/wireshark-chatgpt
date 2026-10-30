# Wireshark WSLua Conventions

This file records durable WSLua/Lua implementation conventions extracted from upstream Wireshark merge-request review. Current upstream source remains authoritative.

## Use contiguous integer keys when Lua sequence semantics are required

Lua reference-table allocation and Lua sequence semantics are different contracts. `luaL_ref()` is appropriate for opaque registry/reference management, but code that relies on table length (`#`) or ordered append/clear behavior should maintain an actual contiguous integer-key sequence instead of depending on implementation details of the reference freelist.

Merged MR !24348, authored and merged by John Thacker, changes WSLua test-library bookkeeping for Lua 5.5 compatibility. The old code used `luaL_ref()` to populate a table and later treated that table as a sequence; changes in Lua's reference freelist behavior made that assumption unsafe. The accepted code explicitly appends entries at consecutive integer indices because the usage is append-only followed by clear-all.

**Implementation rule:** decide whether a Lua table is an opaque reference store or a sequence. If Wireshark code relies on length/order/contiguity, maintain consecutive integer keys directly; do not infer sequence properties from `luaL_ref()` allocation behavior.

**Confidence:** Very high. Merged compatibility fix authored and merged by John Thacker, with the semantic mismatch identified explicitly in the change rationale.

## Preserve structured Lua dissector errors and stack discipline

An error in a Lua dissector should not leave analysts with only a flattened, context-free error message. The Lua protected-call error handler is a suitable place to preserve the stack trace in packet-analysis output, provided its Lua stack indexes and return contract remain correct.

Merged master MR !9891 adds an error handler for WSLua dissectors. For Lua 5.2 and newer, it attaches the original error as expert information and a “Lua Traceback” protocol subtree with individual traceback lines. It adjusts `lua_pcall`'s error-handler stack index so the traceback is added without changing the user-visible error-message result; older Lua paths continue to report the error without the newer traceback capability.

**Implementation rule:** report script failures as expert diagnostics and, where the scripting runtime supports it, expose structured traceback context in the packet tree. When installing a Lua error handler, explicitly document/test the positions of handler, function, arguments, and error result in the Lua stack; guard version-specific APIs.

**Confidence:** High. Merged master implementation by Gilbert Ramirez, accepted by Anders Broman; no substantive additional maintainer design discussion in the corpus snapshot.
