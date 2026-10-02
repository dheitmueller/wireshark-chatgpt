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


## Reserve the worst-case encoded expansion before consuming the next input unit

For bounded escaping/encoding, the capacity proof must use the largest output representation that one input unit can produce, plus the terminating NUL. Checking only the ordinary one-byte case or checking after the expansion is written can still overrun the buffer.

Merged master MR !8302, authored and merged by Gerald Combs after a Coverity overrun report, fixes XML escaping by deriving a flush limit from the fixed buffer size and the longest entity expansion. The code ensures enough room remains for the next escaped character and the NUL before it consumes that input byte, then flushes and resets the output offset when the limit is reached.

**Implementation rule:** determine the maximum output bytes produced by one input unit, include fixed suffix/terminator requirements, and prove that capacity before performing the write. For fixed buffers, flush before the next expansion can cross the bound; for growable buffers, grow before writing.

**Confidence:** Very high. Merged master memory-safety fix by Gerald Combs motivated by a concrete static-analysis overrun.


## Reserve terminator capacity separately from logical payload length

When a capture block stores a byte sequence plus an implementation-added NUL terminator, ensure capacity for `payload_length + 1` without accidentally changing the logical payload length, and index trailing-byte checks from the last valid payload byte.

Merged master MR !1822, authored by Gerald Combs, fixes pcapng systemd-journal block handling by replacing a logical buffer-length increase with `ws_buffer_assure_space(..., entry_length + 1)`, checking trailing NULs at `entry_length - 1`, and then writing the terminator at `entry_length`. Guy Harris-authored stable backports !1825 and !1826 carry the identical correction.

**Capacity rule:** spare storage for a terminator is capacity, not protocol data. Grow/assure the backing allocation without inflating the externally meaningful record length unless the format actually includes that terminator.

**Indexing rule:** if `length` counts payload bytes, the final payload byte is `length - 1`; `buffer[length]` is the first byte beyond the logical payload and is appropriate for a separately reserved terminator only after capacity has been proven.

**Confidence:** Extremely high. Merged master fix plus Guy Harris-authored stable backports.
