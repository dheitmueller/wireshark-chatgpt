# Wireshark TVB Copy and Allocation Conventions

This file records durable conventions for copying packet-backed bytes out of a `tvbuff_t`. Current upstream TVB APIs remain authoritative.

## Prefer bounds-checked duplication before allocating packet-sized storage

When copying a contiguous packet region into newly allocated memory, prefer a TVB helper that validates the requested range before allocating it rather than allocating from a packet-derived length first and copying second.

Merged master MR !24387 changes RTPS paths from `wmem_alloc()` followed by `tvb_memcpy()` to `tvb_memdup()`. John Thacker's companion merged master MR !24390 adds the recommendation directly to the `tvb_memcpy()` API documentation: `tvb_memdup()` should be preferred when possible because it checks that the requested bytes are available before allocation. The release-4.6 and release-4.4 backports !24391 and !24392 preserve the same safety change.

This ordering matters especially when the requested size comes from packet data and has undergone arithmetic such as subtracting a fixed header size. A malformed value can underflow or otherwise become implausibly large; allocate-then-copy may waste substantial memory or fail before the TVB bounds check ever rejects the request.

**Implementation rule:** for a newly owned copy of a contiguous TVB range, use `tvb_memdup()` when its ownership/lifetime contract fits. Avoid `wmem_alloc(size)` + `tvb_memcpy(..., size)` when `size` is packet-derived merely to obtain a copy; validate the range before committing an allocation.

**Confidence:** Very high. Merged master implementation plus an explicit John Thacker-authored API-documentation change and two stable-branch backports.