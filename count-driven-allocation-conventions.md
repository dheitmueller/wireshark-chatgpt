# Wireshark Count-Driven Allocation Conventions

This file records durable rules for allocating collections whose sizes are derived from untrusted packet data. Current upstream source remains authoritative.

## Grow count-driven collections as elements are validated

An untrusted element count can be syntactically valid while grossly overstating how many complete elements are actually present. If element sizes vary, a later TVBuff bounds exception may stop parsing correctly but cannot undo a huge allocation already made from the advertised count, especially when that allocation lives at file scope.

Merged master MR !9830, authored by John Thacker, fixes RTPS type dissection after fuzzing produced messages with a bogus 32-bit element count. The old code allocated the entire file-scope array before parsing any elements, which could exhaust memory even though a BoundsError would eventually stop dissection. The accepted implementation creates a `wmem_array_t` with a bounded initial capacity, appends elements only as they are successfully parsed, and adds `wmem_array_finalize()` to trim and return the finished raw array. The RTPS maintainer explicitly tested the change with large legitimate types.

**Implementation rule:** when a count comes from untrusted input and the parser cannot cheaply prove that all counted elements are present, avoid committing `count * sizeof(element)` storage before validation. Start with a reasonable bounded capacity and grow with successfully decoded elements. This protects implementation resources without imposing an arbitrary protocol limit on legitimate large messages.

**Review rule:** broad-lifetime allocations such as file-scope arrays deserve extra scrutiny because speculative over-allocation survives beyond the packet that triggered it.

**Confidence:** Very high. Merged master fuzz/resource fix authored by John Thacker with explicit large-type regression testing.
