# Wireshark Wiretap Reader Validation Conventions

This file records durable conventions for validating capture-file structures and propagating reader failures. Current upstream source remains authoritative.

## Validate length relationships before seeking or deriving substructure sizes

Capture-file headers often contain multiple related lengths. It is not enough to check each value in isolation; validate the ordering and containment relationships the format requires before subtracting lengths, seeking past optional header data, or trusting an object extent.

Merged release MRs !15204 and !15205, authored and merged by Guy Harris, harden the BLF reader. The accepted code rejects a header shorter than the fixed base header, rejects an object whose total length is smaller than its declared header length, distinguishes unsupported header types from malformed-file errors, and checks `file_seek()` results instead of continuing after failed repositioning. The scan helper was also changed to propagate `err` and `err_info` to its caller, and the file-open path cleans up and fails when the scan fails rather than silently proceeding with incomplete state.

**Implementation rule:** before computing `payload_len = object_len - header_len`, skipping an extension, or seeking to a derived offset, prove the required structural inequalities first. Treat malformed lengths as input errors, not as values to coerce into a plausible range.

**Error-contract rule:** a helper that can fail while scanning or indexing a capture must return enough error information for the opener/caller to distinguish malformed input, unsupported format features, EOF, and I/O failures. Do not discard a lower-level seek/read error and later report a generic parse failure.

**Lifecycle rule:** if an open-time scan fails after allocating per-file state, unwind that state before returning the error. An opener must not leave a partially initialized reader object behind.

**Review implication:** malformed-file tests should include undersized headers, total lengths smaller than header lengths, unsupported header variants, truncated reads, and failed seeks where practical. Review every subtraction and derived seek against the validated invariants that make it safe.

**Confidence:** Extremely high. Both accepted fixes were authored and merged by Guy Harris, and !15204 explicitly changes the reader's validation, error propagation, seek checking, and cleanup behavior.

## Treat unexpected EOF during packet reread as a read failure

Random-access packet reread has a stronger precondition than sequential iteration: the packet was already discovered successfully, so an end-of-file result at its recorded offset is not the normal end of iteration.

During review of merged master MR 4353, which adds a default seek-and-read implementation for Lua FileHandlers, Guy Harris says that this case should report `WTAP_ERR_SHORT_READ`. He also notes that the rule is general enough to enforce at the shared Wiretap layer so C and Lua readers receive the same behavior.

**Reader-contract rule:** distinguish normal sequential EOF from unexpected EOF while rereading a previously discovered packet.

**Layering rule:** when an error-semantic invariant applies to every reader backend, prefer enforcing it at the shared abstraction boundary rather than separately in each implementation.

**Confidence:** High as direct senior-maintainer review guidance from Guy Harris. The specific merge supplies the fallback mechanism; the shared-layer placement is review guidance rather than proof of the exact historical implementation.


## Return error text with the ownership required by the Wiretap API

An error-output parameter is also a memory-ownership contract. Returning a pointer to static storage through an output that callers release can make the error path itself invalid even though the message text is constant.

Merged master MR 4284 changes the USBDump reader's `err_info` assignment from a string literal to allocated GLib storage. The identical fix was carried to release-3.4 and master-3.2 by MRs 4285 and 4286, both authored by Guy Harris.

**Error-contract rule:** when a Wiretap `gchar **err_info` path transfers ownership to a caller that frees the string, allocate the diagnostic with the expected allocator on every path. Do not return a literal or borrowed pointer merely because the message has static contents.

**Confidence:** Extremely high. Merged master fix plus two maintained-branch backports authored by Guy Harris.
