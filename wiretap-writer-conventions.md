# Wireshark Wiretap Writer Conventions

This file records durable conventions for capture-format writers and their failure paths. Current upstream wiretap code remains authoritative.

## Choose traversal APIs that permit writer failures to stop and propagate

When serializing a collection, the traversal mechanism must permit the first write/validation failure to stop further output and return the real error to the caller. A callback API that cannot return failure is a poor fit for fallible serialization; use explicit iteration when necessary. Validate the complete serialized record size before copying variable-length components into fixed storage.

Merged MR !23930, authored and merged by John Thacker, fixes K12 source-descriptor writing by checking the full record length against the fixed record buffer before the `memcpy` operations and by replacing `g_hash_table_foreach()` with `GHashTableIter`. The old callback could not naturally propagate a failed write; the explicit loop can stop immediately and return `err`/`err_info` to the caller. The change was driven by a supplied PoC and also corrected previously acknowledged error-handling debt.

**Implementation rule:** structure capture writers around fallible operations. Perform all size/capacity validation before writes into fixed storage, and use iteration/control flow that can terminate on the first error rather than discarding or deferring I/O failures.

**Confidence:** Very high. Merged master writer hardening authored and merged by John Thacker, with a concrete reproducer.

## When outer record length absorbs alignment padding, prove payload framing remains unambiguous

Some capture formats require alignment between container records while also recording a length that spans the final padding. Backpatching the previous record's outer length can be a valid way to meet that contract, but only when every affected record type has an independent way to determine where meaningful payload ends. Otherwise padding bytes can become indistinguishable from data.

Merged master MR !23608 changes the BLF writer so closing a log container pads the last object/block to a 4-byte boundary and backpatches the stored object/block length. The change does not rely on alignment alone: the implementation was accompanied by an audit of writable BLF object layouts to establish that variable payloads carry their own lengths or have fixed extents. Validation exercised CAN, CAN FD, CAN XL, LIN, FlexRay, and Ethernet combinations at adverse offsets, round-tripped the files, checked Vector-tool interoperability, and benchmarked a 10-million-packet workload to ensure the writer-path change did not introduce meaningful overhead. Its supported-branch precursor !23657 independently confirms the accepted format fix.

**Implementation rule:** if a writer makes an enclosing length include alignment bytes, first prove that readers can still identify the true payload boundary for every record type that can occupy that position. Test the worst alignment cases and heterogeneous record families, and use independent readers/tools or round trips when available.

**Review rule:** do not accept “padding is harmless” as an assumption when the padding lies inside a recorded object length. Review both the container framing and the nested record's own length semantics.

**Confidence:** Very high. Merged master format-correctness change with broad record-type validation and an accepted stable-branch counterpart.

## Refresh writer mappings when interface metadata can grow during a dump

Capture-writer state derived from interface description blocks is not necessarily fixed at writer initialization. If the wiretap layer can notify a writer that a new IDB has appeared while dumping, any cached interface-to-format mapping must incorporate that IDB before later packets are serialized.

Merged master MR !23488, authored and merged by Guy Harris, expands the BLF interface mapping when a new IDB is supplied to the dumper. The existing implementation built the mapping only from the initial interface set, so packets associated with later IDBs could not be mapped correctly. Stable-branch counterpart !23511 independently carries the accepted fix.

**Implementation rule:** when a capture format writer caches metadata derived from IDBs or another collection that wiretap permits to grow during output, handle the writer's metadata-update callback by extending or rebuilding that derived state. Do not assume the interface set observed at open time is complete.

**Confidence:** Extremely high. Merged master architecture fix authored and merged by Guy Harris, with an accepted stable-branch backport.

## Keep defensive result checks even after repairing the invariant that normally prevents failure

Fixing the root cause of an impossible-state failure does not make a nearby fallible-operation check redundant. If a writer/helper can report failure, keep checking that result when doing so converts an unexpected invariant violation into a controlled internal error instead of a process crash. This gives future regressions a diagnosable failure path rather than relying on the invariant remaining perfect forever.

Merged MR !23455 adds the missing success check around BLF interface mapping. Guy Harris explicitly stated that this check should still be committed even with the underlying !23488 interface-mapping fix, because the check turns the condition into an internal error rather than a crash. The MR was subsequently merged by Guy.

**Implementation rule:** distinguish root-cause prevention from defensive containment. Repair the state/invariant bug, but retain inexpensive checks on fallible writer operations when they provide a defined error path for unexpected future violations.

**Confidence:** Extremely high. Direct Guy Harris guidance on a merged wiretap crash fix, with the root-cause fix independently merged.

## Declare required seeking before a writer is allowed to start

A file format that must seek while finalizing output must advertise that requirement in its file-type metadata. The framework should reject incompatible destinations up front rather than letting a write progress until a later seek/tell operation fails.

Merged MR !23045 fixes the BLF writer by setting `writing_must_seek` because BLF must seek back to update its file header with the final file size. Without that declaration, compressed output could begin successfully and fail only when the writer eventually attempted a seek or tell. Merged follow-up !23049 documents the contract explicitly: modules using `wtap_dump_file_seek()` should set `writing_must_seek`, and wiretap does not support seeking while writing compressed streams.

Merged !23047 additionally checks the return from `wtap_dump_file_tell()` even though the corrected `writing_must_seek` invariant should make failure unlikely. That independently corroborates the defensive-result-check rule above.

**Implementation rule:** if correct serialization requires random access, declare that requirement in the writer's file-type metadata and let wiretap reject unsupported/non-seekable destinations before output begins. Still check individual seek/tell operations for failure rather than treating the capability declaration as proof that every operation must succeed.

**Confidence:** Very high. The capability fix and its documentation were merged master changes authored by John Thacker and accepted by maintainers.