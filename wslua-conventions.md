# Wireshark WSLua Conventions

This file records durable WSLua/Lua implementation conventions extracted from upstream Wireshark merge-request review. Current upstream source remains authoritative.

## Use contiguous integer keys when Lua sequence semantics are required

Lua reference-table allocation and Lua sequence semantics are different contracts. `luaL_ref()` is appropriate for opaque registry/reference management, but code that relies on table length (`#`) or ordered append/clear behavior should maintain an actual contiguous integer-key sequence instead of depending on implementation details of the reference freelist.

Merged MR !24348, authored and merged by John Thacker, changes WSLua test-library bookkeeping for Lua 5.5 compatibility. The old code used `luaL_ref()` to populate a table and later treated that table as a sequence; changes in Lua's reference freelist behavior made that assumption unsafe. The accepted code explicitly appends entries at consecutive integer indices because the usage is append-only followed by clear-all.

**Implementation rule:** decide whether a Lua table is an opaque reference store or a sequence. If Wireshark code relies on length/order/contiguity, maintain consecutive integer keys directly; do not infer sequence properties from `luaL_ref()` allocation behavior.

**Confidence:** Very high. Merged compatibility fix authored and merged by John Thacker, with the semantic mismatch identified explicitly in the change rationale.


## Preference mutation from Lua must preserve preference-system bookkeeping

Merged MR !792 adds WSLua preference read and mutation helpers. Peter Wu challenged the global-state mutation capability and asked for a concrete use case and tests; Stig Bjørlykke supplied both. The accepted implementation uses the preference subsystem's typed setters, propagates module change flags, handles missing and unsupported preferences through defined Lua results or errors, and adds Lua tests for the new API.

**Implementation rule:** when Lua can mutate application-wide state, route the change through the owning subsystem's canonical setters and change/effect bookkeeping rather than writing underlying storage directly.

**Review/testing rule:** require a concrete extension use case and tests across supported value classes and error cases before broadening a scripting API that can change global application behavior.

**Confidence:** High. Merged API addition with direct Peter Wu design review and tests added during review.
