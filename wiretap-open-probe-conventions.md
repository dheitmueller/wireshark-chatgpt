# Wiretap open/probe conventions

## Keep file recognition at the open boundary

A Wiretap open routine has a special responsibility that ordinary record-reading code does not: it may conclude that the input is not its format. Once the file has been accepted, the reader should treat malformed headers, short reads, and invalid blocks as ordinary read or bad-file failures rather than reusing a "not my format" state.

Guy Harris's merged !2349 moves initial pcapng Section Header Block recognition into `pcapng_open()`. The normal `pcapng_read_block()` path no longer has to support both probing and established-file reading, so its result becomes a straightforward success/failure value and its error handling is materially simpler. Release-3.4 !2350 carries the same design. Earlier !2335/!2342 are useful transitional history.

**Implementation rule:** put recognition-only branching in the format opener. After acceptance, keep record readers in the established-format state machine and report invalid input using normal Wiretap error semantics.

**Confidence:** Extremely high. Merged master design and stable backport authored by Guy Harris.
