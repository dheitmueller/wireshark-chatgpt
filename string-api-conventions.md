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

## Formatting helpers that write into caller storage must receive the caller's capacity

A formatting helper cannot safely assume that every destination has the canonical field-label size. The same formatter may be used for a normal protocol-tree label, a larger custom-column buffer, or some other caller-owned destination. Hard-coding one capacity in the helper makes the implementation depend on an accidental calling context and can turn a legitimate larger or differently sized destination into an overflow/truncation bug.

Merged master MR !20320, authored and merged by Martin Mathieson after a crash with many O-RAN I/Q values, changes floating-point label helpers to take `label_str_size` explicitly and passes the actual capacity from each caller. Normal item-label callers pass `ITEM_LABEL_LENGTH`; the custom-column path passes its larger buffer size. During review, Guy Harris asked whether the code should format into an intermediate buffer and use `label_fill()`; that prompted examination of where `label_fill()` already occurs and expansion of the same size-aware treatment to the IEEE 11073 float formatter.

**Implementation rule:** if a helper writes into memory owned by its caller, make destination capacity part of the helper contract and propagate the real size at every call site. Do not infer capacity from a conventional constant unless the type/API itself guarantees that exact storage size.

**Review rule:** when fixing one overflow in a family of formatting helpers, audit sibling helpers and all callers for the same hidden-size assumption rather than applying a one-site bound check.

**Confidence:** Extremely high. Merged crash fix plus direct Guy Harris review; the accepted change generalized the size contract across both affected floating-point formatting paths.

## Do not hide generic UTF-8 string limits inside an octet-counted builder

A generic string builder should not silently enforce a caller-independent maximum measured in raw octets when the object it builds is UTF-8 text. Such a ceiling complicates append paths and can cut a multibyte character at the boundary. If a caller needs a presentation or protocol limit, make that policy explicit at the caller or at a semantic truncation boundary.

Merged MR !8964, authored and merged by João Valverde, removes the maximum-size parameter from `wmem_strbuf` and renames the sized constructor so its remaining purpose—initial sizing—is clear. Gerald Combs explained that the old maximum was intended to limit damage from runaway appending such as a gigantic item label, but accepted that the generic byte ceiling caused more problems than it solved. Callers can use `wmem_strbuf_truncate()` explicitly when a real limit is needed.

**API rule:** separate initial-capacity/performance hints from semantic maximum length. Do not make a generic UTF-8 container enforce an implicit maximum by byte count unless the API also defines safe character-boundary behavior and a compelling shared policy.

**Review rule:** when adding a resource guard to text construction, ask whether the limit belongs to the data structure or to one consumer. Prefer an explicit consumer-side limit when call sites have different semantics.

**Confidence:** Very high. Merged core API simplification with the original resource-safety motivation discussed directly by Gerald Combs.
