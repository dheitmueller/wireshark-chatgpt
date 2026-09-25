# TVBuff Ownership Conventions

## Prefer child decompression helpers when the decompressed TVBuff should follow a parent lifetime

The raw tvb_uncompress family returns a separately owned TVBuff. If the caller neither frees it nor attaches it to another TVBuff chain, the wrapper and its backing allocation leak. For the common dissector case where decompressed data is only a derived view of the current packet, use the child variant so ownership is represented structurally.

Merged master MR !9236 audits several dissectors after Valgrind showed leaks from tvb_uncompress calls and converts MySQL, GELF, MCPE, multipart, RTPS, and SIP to tvb_child_uncompress where the decompressed buffer belongs to an existing TVBuff. It also updates the API documentation to say explicitly that raw uncompress results must be freed or attached. John Thacker's review states that the set of callers that actually need the non-child lifetime is small and recommends the child helper in the documentation.

**Implementation rule:** choose raw decompression only when the returned TVBuff genuinely needs an independent lifetime and make its release or attachment explicit. When its lifetime should match an existing packet or parent TVBuff, use tvb_child_uncompress (or the format-specific child variant) so normal and exception paths share the same ownership.

**Review rule:** when auditing a TVBuff-producing helper, trace ownership of the wrapper as well as the backing bytes. A valid pointer passed to a subdissector is not evidence that somebody will eventually free it.

**Confidence:** Extremely high. Merged cross-dissector leak fix with Valgrind reproductions and explicit ownership guidance from John Thacker; it independently corroborates the later child-TVBuff exception-safety convention.
