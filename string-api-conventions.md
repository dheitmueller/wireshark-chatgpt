# Wireshark String API Conventions

This file records durable C/string API conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Keep destination capacity distinct from source character count

String helpers that look similar can give their size argument different meanings. Do not pass the length of the source token to an API whose contract requires the full capacity of the destination buffer, and do not infer termination semantics from another API with a similar name.

Merged MR !24254, authored by John Thacker, fixes the Snort configuration parser after `g_strlcpy()` had effectively been treated like a bounded-format/source-count operation. `g_strlcpy(dest, src, size)` expects the total destination-buffer size and always reserves space for termination when size is nonzero. By contrast, `g_strndup(src, n)` takes the number of source bytes to duplicate and provides its own trailing NUL. The accepted fix uses the token length with `g_strndup()` when copying a delimited slice, and the actual static-buffer capacity with `g_strlcpy()` when copying into a fixed buffer.

The same change also highlights an important parser detail: if a token reader can return the rest of an entire line rather than an already NUL-delimited token, callers must either provide the delimiter themselves or use an explicit-length duplication API. A pointer into a larger line is not automatically a C string ending at the logical token boundary.

**Implementation rule:** annotate mentally (or in variable naming) whether a length means source bytes, logical token width, or destination capacity. Match that quantity to the called API's documented contract instead of substituting one for another.

**Parsing rule:** when a logical token is only length-delimited inside a larger buffer, use an explicit-length string constructor or terminate a copy at that boundary before passing it to C-string APIs.

**Confidence:** Very high. Merged parser correctness/safety fix authored by John Thacker; the MR description explicitly distinguishes the affected API contracts.

## Only repair a truncated multibyte tail when the formatter actually reached the buffer end

The return value and size arguments of `snprintf()` have different semantics from the initialized contents of the destination buffer. A helper that repairs a possibly truncated UTF-8 tail must only inspect the tail when formatting actually wrote through the end of the initialized string; otherwise bytes beyond the terminating NUL may be uninitialized even though they lie inside the allocated array.

Merged MR !22947, authored and merged by John Thacker, fixes P4RPC by calling `ws_utf8_truncate()` only when `snprintf()` reached the end of the destination buffer. The prior code could ask the UTF-8 truncation helper to inspect uninitialized bytes. The MR explicitly notes how easy it is to confuse which string-API quantities include the NUL terminator and which do not.

**Implementation rule:** after bounded formatting, use the formatter's documented return contract to distinguish complete output from truncation before examining or repairing the destination tail. Never infer initialized extent from buffer capacity alone, and keep payload length, destination capacity, and NUL-terminator accounting as separate quantities.

**Confidence:** Very high. Merged master undefined-behavior fix authored and merged by John Thacker.