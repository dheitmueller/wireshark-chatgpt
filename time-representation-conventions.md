# Time Representation Conventions

## Preserve the full width of `time_t`-backed timestamp seconds

Merged master MR !2854, authored by Guy Harris, removes casts through `guint32` when assigning values to `nstime_t.secs`. That member is a `time_t`, whose width is platform-dependent and is not guaranteed to be 32 bits. Narrowing through an unsigned 32-bit type creates a Y2038-style truncation even on platforms where `time_t` is wider.

**Implementation rule:** keep timestamp seconds in the natural `time_t`/destination type domain. Do not introduce a narrower intermediate cast unless the protocol itself imposes that range and the narrowing is explicitly checked.

**Review rule:** when touching timestamp conversions, trace the width and signedness of the source, intermediate, and destination types separately. A cast that quiets a warning can silently reduce the supported time range.

**Confidence:** extremely high. Merged master portability/correctness fix authored by Guy Harris.
