# Wireshark Buffer-Length Return Conventions

This file records durable conventions for interpreting and propagating buffer-copy lengths in Wireshark code. Current upstream APIs and source remain authoritative.

## Distinguish the logical source length from the bytes actually materialized in the destination

String-copy helpers can deliberately return a value different from the number of bytes placed in the output buffer. In particular, `strlcpy()`-style APIs report the length that would have been copied if the destination were large enough. That return value is useful for truncation detection, but it is not safe to reuse as an append offset or as the amount of initialized destination data after truncation.

Merged master MR !11445, authored and merged by John Thacker, fixes a crash while constructing packet-list columns containing many long string fields. `ws_label_strcpy()` follows `strlcpy()` semantics and can therefore return a length larger than the destination when truncation occurs. Its caller, `proto_item_fill_display_label()`, has a different contract: callers need the number of bytes actually present in the label so they can continue appending safely. The accepted fix detects truncation and returns `label_str_size - 1` (or zero for a zero-sized destination) as the actual copied length.

**Implementation rule:** before propagating the return from a bounded-copy helper, determine which length domain the caller expects: source/logical length, required capacity, or actual bytes produced. If the caller will use the result as a destination offset, writable-length value, or count of initialized bytes, translate a would-have-written result to the actual materialized length after truncation.

**Review rule:** treat length-return semantics as part of the API contract. A helper and its caller can both be individually reasonable yet incompatible if one reports required/logical length while the other interprets the value as bytes written. Check truncation cases explicitly whenever the returned length feeds later pointer arithmetic or concatenation.

**Confidence:** Extremely high. Merged master crash fix authored and merged by John Thacker, with the `strlcpy()`-style return-value mismatch stated directly in the commit rationale.
