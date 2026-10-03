# Unsigned Range and Allocation Conventions

## Validate ordering before deriving unsigned sizes

Merged master MR !80, authored and merged by Gerald Combs, fixes USB HID parsing where packet-controlled Usage Minimum and Usage Maximum values were used as `usage_max - usage_min` for `wmem_array_grow()`. A reversed or equal range could otherwise produce an invalid or enormous derived size. The accepted fix rejects `usage_min >= usage_max` before subtraction.

**Implementation rule:** validate ordering, representability, and protocol bounds before unsigned subtraction, allocation growth, iteration counts, or pointer arithmetic. Do not rely on the allocator to reject a nonsensical derived size.

**Testing rule:** malformed-input tests should include reversed, equal, and extreme endpoints around every packet-controlled range that feeds resource sizing.

**Evidence weight:** Very high. Merged master resource-safety fix by Gerald Combs.
