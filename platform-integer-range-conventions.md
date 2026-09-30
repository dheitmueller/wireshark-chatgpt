# Wireshark Platform Integer-Range Conventions

Merged MR !7902 fixes 32-bit Linux timestamp handling. Guy Harris pointed out that the proposed `WTAP_NSTIME_SECS_MAX` name sounded like a universal timestamp maximum, while the code actually needed the maximum positive seconds value that could be stored in a 32-bit representation given the platform's `time_t` width. The accepted constant was renamed `WTAP_NSTIME_32BIT_SECS_MAX`.

**Rule:** name range constants for the actual representation or protocol constraint being enforced, not for a broader type concept.

**Portability rule:** derive bounds from the width and signedness of the receiving representation rather than assuming the host type has one fixed width across supported platforms.

**Confidence:** Very high. Merged portability fix with direct Guy Harris review and approval.

## Keep Qt size-domain values wide until an API actually requires int

Merged !6486 addresses Qt 6's `qsizetype` migration. Jaap Keuter explicitly asked that narrowing use `static_cast`, and the accepted code keeps counts/indexes as `qsizetype` where practical while making conversions explicit at APIs that still take `int`. The adjacent merged !6500 discussion independently calls broad long-long-to-int casts undesirable.

**Qt portability rule:** preserve the library's native size/index type through calculations and narrow only at a real narrower API boundary. Use an explicit C++ cast and verify the range assumption instead of relying on implicit or C-style conversion merely to silence a compiler warning.

**Confidence:** Very high. Direct Jaap Keuter review incorporated into merged code, corroborated by Gerald Combs and Roland Knall discussion in !6500.


## Size types must respect the narrowest external I/O API, not just pointer width

Merged master MR !4085, authored by Guy Harris, caps Wiretap input/output buffers at 2^30 and uses `guint` for their sizes and offsets. The rationale is concrete: MSVC `_read()` returns `int`, while zlib uses unsigned-int-sized availability fields. Closed !4081 and !4082 preserve Guy's accompanying review that simply switching everything to pointer-sized `gsize/gssize` would not remove those backend limits.

**Portability rule:** select buffer-size and offset types from the full API chain that consumes them, and enforce a bound that makes every required conversion and result representable. A wider host-side type cannot extend a narrower system/library interface.

**Confidence:** Extremely high. Accepted merged implementation authored by Guy Harris, with the rejected alternatives documenting the same constraint.
