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