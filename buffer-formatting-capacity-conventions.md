# Buffer Formatting and Capacity Conventions

This file records durable Wireshark conventions for formatting text into bounded buffers when helper APIs report capacities, maximum lengths, or lengths including terminators. Current upstream source remains authoritative.

## Normalize length contracts before doing capacity arithmetic

A formatter may expose an upper bound rather than an exact produced length, and that bound may include the terminating NUL. Callers must normalize those semantics before comparing against destination capacity or using the result as a text length. Mixing “maximum bytes including NUL”, “actual characters”, and “available destination bytes” is a common source of off-by-one and truncation bugs.

Merged master MR !11098, authored, approved, and merged by Guy Harris, rewrites `address_with_resolution_to_str_buf()`. The address-type callback's `addr_str_len()` result is treated explicitly as an upper bound that includes the terminating NUL, so the accepted code subtracts one before reasoning about the address text itself. It then uses the actual copied resolved-name length and separately accounts for the fixed `" ("`, `")"`, and NUL overhead when the address is appended in parentheses.

**Implementation rule:** before capacity arithmetic, write down which length domain each value uses: exact produced bytes, maximum output bytes, characters excluding NUL, size including NUL, or destination capacity. Convert to one domain deliberately rather than relying on coincidental equality for ordinary inputs.

**Boundary rule:** make the terminating NUL part of the capacity proof. A destination is large enough only when the complete representation, including punctuation/separators and the terminator, fits according to the called API's contract.

## Branch on semantic emptiness, not on a special type that happens to be empty today

Presentation decisions should be based on the property that actually matters. In the same !11098 change, the old code special-cased `AT_NONE` to avoid appending an address. Guy's accepted rewrite instead asks whether the address string's maximum non-NUL length is zero. That naturally handles `AT_NONE` and any other address type that can legitimately format to an empty string.

The rewrite also treats an empty resolved name as a distinct formatting case: when there is no name, it emits just the address instead of constructing a parenthesized suffix and uses the capacity formula appropriate to that representation.

**Design rule:** if formatting behavior depends on whether a representation is empty, test the representation/property itself rather than enumerating one producer/type currently known to yield an empty value. This makes the formatter robust to additional address types and keeps layout logic aligned with user-visible semantics.

**Confidence:** Extremely high. The merged master rewrite was authored, approved, and merged by Guy Harris and contains explicit comments documenting the upper-bound/NUL and formatting-capacity contracts.
