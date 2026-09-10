# Wireshark String API Conventions

This file records durable C/string API conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Keep destination capacity distinct from source character count

String helpers that look similar can give their size argument different meanings. Do not pass the length of the source token to an API whose contract requires the full capacity of the destination buffer, and do not infer termination semantics from another API with a similar name.

Merged MR !24254, authored by John Thacker, fixes the Snort configuration parser after `g_strlcpy()` had effectively been treated like a bounded-format/source-count operation. `g_strlcpy(dest, src, size)` expects the total destination-buffer size and always reserves space for termination when size is nonzero. By contrast, `g_strndup(src, n)` takes the number of source bytes to duplicate and provides its own trailing NUL. The accepted fix uses the token length with `g_strndup()` when copying a delimited slice, and the actual static-buffer capacity with `g_strlcpy()` when copying into a fixed buffer.

The same change also highlights an important parser detail: if a token reader can return the rest of an entire line rather than an already NUL-delimited token, callers must either provide the delimiter themselves or use an explicit-length duplication API. A pointer into a larger line is not automatically a C string ending at the logical token boundary.

**Implementation rule:** annotate mentally (or in variable naming) whether a length means source bytes, logical token width, or destination capacity. Match that quantity to the called API's documented contract instead of substituting one for another.

**Parsing rule:** when a logical token is only length-delimited inside a larger buffer, use an explicit-length string constructor or terminate a copy at that boundary before passing it to C-string APIs.

**Confidence:** Very high. Merged parser correctness/safety fix authored by John Thacker; the MR description explicitly distinguishes the affected API contracts.
