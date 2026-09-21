# Empty-value API conventions

This file records durable conventions for APIs where absence and a valid zero-length value are distinct states. Current upstream source remains authoritative.

## Do not collapse a valid empty buffer into a NULL/absent result

A non-NULL pointer with zero length can be a legitimate value, not an error and not the same semantic state as a NULL pointer. Conversion and parser helpers must preserve that distinction when callers depend on being able to represent an empty field explicitly.

Merged master MR !14799, authored and merged by John Thacker, restores the historical `bytes_to_str*()` behavior in which a non-NULL byte buffer with length zero converts to an allocated empty string. Wireshark had supported that behavior before 3.6 and callers such as UAT handling relied on it. The accepted fix keeps the NULL-pointer guard as the absent/error-like case but handles `src_size == 0` separately by returning `""`. Release-4.2 MR !14806 carries the same correction.

**API rule:** treat pointer validity and content length as independent dimensions unless the API contract explicitly says otherwise. If `NULL` means no value while `(non-NULL, 0)` means a present-but-empty value, preserve both states through conversions rather than using a combined `!ptr || !len` shortcut.

**Compatibility rule:** before tightening an old utility helper around empty input, check historical behavior and real callers. A defensive-looking zero-length rejection can be an API regression if valid-empty values were previously accepted and serialized/configured consumers rely on that distinction.

**Confidence:** Very high. Merged master compatibility fix authored and merged by John Thacker, with an accepted stable-branch backport and a concrete UAT dependency noted in the MR.
