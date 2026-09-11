# Wireshark Wiretap Writer Conventions

This file records durable conventions for capture-format writers and their failure paths. Current upstream wiretap code remains authoritative.

## Choose traversal APIs that permit writer failures to stop and propagate

When serializing a collection, the traversal mechanism must permit the first write/validation failure to stop further output and return the real error to the caller. A callback API that cannot return failure is a poor fit for fallible serialization; use explicit iteration when necessary. Validate the complete serialized record size before copying variable-length components into fixed storage.

Merged MR !23930, authored and merged by John Thacker, fixes K12 source-descriptor writing by checking the full record length against the fixed record buffer before the `memcpy` operations and by replacing `g_hash_table_foreach()` with `GHashTableIter`. The old callback could not naturally propagate a failed write; the explicit loop can stop immediately and return `err`/`err_info` to the caller. The change was driven by a supplied PoC and also corrected previously acknowledged error-handling debt.

**Implementation rule:** structure capture writers around fallible operations. Perform all size/capacity validation before writes into fixed storage, and use iteration/control flow that can terminate on the first error rather than discarding or deferring I/O failures.

**Confidence:** Very high. Merged master writer hardening authored and merged by John Thacker, with a concrete reproducer.
