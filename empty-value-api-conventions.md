# Empty-value API conventions

This file records durable conventions for APIs where absence and a valid zero-length value are distinct states. Current upstream source remains authoritative.

## Do not collapse a valid empty buffer into a NULL/absent result

A non-NULL pointer with zero length can be a legitimate value, not an error and not the same semantic state as a NULL pointer. Conversion and parser helpers must preserve that distinction when callers depend on being able to represent an empty field explicitly.

Merged master MR !14799, authored and merged by John Thacker, restores the historical `bytes_to_str*()` behavior in which a non-NULL byte buffer with length zero converts to an allocated empty string. Wireshark had supported that behavior before 3.6 and callers such as UAT handling relied on it. The accepted fix keeps the NULL-pointer guard as the absent/error-like case but handles `src_size == 0` separately by returning `""`. Release-4.2 MR !14806 carries the same correction.

**API rule:** treat pointer validity and content length as independent dimensions unless the API contract explicitly says otherwise. If `NULL` means no value while `(non-NULL, 0)` means a present-but-empty value, preserve both states through conversions rather than using a combined `!ptr || !len` shortcut.

**Compatibility rule:** before tightening an old utility helper around empty input, check historical behavior and real callers. A defensive-looking zero-length rejection can be an API regression if valid-empty values were previously accepted and serialized/configured consumers rely on that distinction.

## Allocator or ownership variants should preserve the modeled API's edge semantics

When a Wireshark helper deliberately presents itself as an allocator- or ownership-adjusted version of an established API, matching the source API means matching its valid edge cases as well as its common path. Do not let an implementation convenience silently turn an empty-but-valid collection into an invalid/absent result.

Merged master MR !12720, authored and merged by John Thacker, changes `wmem_strjoinv()` to match `g_strjoinv()` when given a non-NULL, NULL-terminated string array containing no strings: it returns a wmem-allocated empty string rather than `NULL`. The corresponding documentation explicitly distinguishes an empty array from an invalid `NULL` array and also documents that a `NULL` separator is equivalent to an empty separator. João Valverde's review additionally moved the invalid-input check to the project's standard `ws_return_val_if()` form. Merged release-4.2 backport !12722 carries the same behavior and documentation.

**Wrapper rule:** if an API is documented as behaving "as" another API except for allocation or lifetime, treat the modeled API's edge-case semantics as part of the compatibility contract. Preserve distinctions such as empty collection versus invalid pointer, and document those edge cases explicitly so callers do not have to infer them from implementation details.

**Review rule:** when changing a compatibility helper, compare the behavior matrix—not just the main algorithm—against the API it models: invalid pointer, valid empty input, singleton input, separator/default behavior, ownership, and allocation lifetime are all potential compatibility surfaces.

**Confidence:** Extremely high. Both examples are merged master fixes authored and merged by John Thacker with stable-branch backports; !12720 also contains accepted maintainer review on the project's standard invalid-input guard idiom.