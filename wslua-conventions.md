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

## Reject invalid negative lengths before TVBuff range arithmetic in Lua bindings

Merged master MR !9823, authored by Gilbert Ramirez and merged by Martin Mathieson, documents that WSLua `TvbRange` length `-1` means "to end of TVBuff", but rejects any `len < -1` with `luaL_error()` before signed arithmetic and unsigned bounds comparison.

**Implementation rule:** distinguish a documented sentinel from other negative values; validate native-side signed lengths before adding offsets or casting to unsigned. Report script misuse through Lua's error boundary instead of passing a malformed range to TVBuff operations.

**Confidence:** High: merged WSLua correctness fix with maintainer approval.

## Avoid proliferating redundant ByteArray APIs; test binding metadata and signed boundaries

Merged master **!8997** added fixed-width and endian-specific Lua `ByteArray` integer readers. **Stig Bjørlykke** immediately challenged duplicating the existing `ByteArray:tvb():range(...):int()` family, asked for consistent names and generic `(le_)?u?int(offset,length)` / `(le_)?u?int64(offset,length)`, and subsequently reported that **!11757** implemented those naming/API changes. The author argued that `ByteArray:tvb()` is unavailable during initialization and supplied a 100,000-iteration *microbenchmark* showing an approximately 419-fold difference for this particular conversion-heavy path; that figure should not be generalized without representative measurements.

The merged !8997 code's `WSLUA_ARG_ByteArray_get_index_INDEX` macro used another method's identifier, breaking generated `make-wsluarm.py` documentation; **Chuck Craft** traced and linked the correction in **!9342**. Subsequent unit-test work exposed a negative-integer handling bug. These post-merge problems materially narrow the original merge's evidentiary value.

**Rule:** before introducing a scripting API, audit an equivalent `TvbRange` or subset API, execution-phase availability, consistency with the rest of WSLua and realistic performance. Keep generated argument-ID prefixes aligned with the containing method; exercise documentation generation as part of CI. Test unsigned and signed values (including negative values), each supported width/endian, offset/length boundaries, and Lua initialization vs dissector contexts. Prefer later corrections !11757/!9342 to copying the original !8997 interface.

**Confidence:** Extremely high for Stig's maintainer feedback and observed generator failure; medium for performance magnitude outside the supplied microbenchmark.
