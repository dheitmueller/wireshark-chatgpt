# Reassembly Contiguous-Frontier Conventions

This file records durable Wireshark conventions for maintaining the contiguous frontier of an out-of-order stream reassembly. Current upstream source remains authoritative.

## Re-evaluate retained fragments when new data closes a gap

An out-of-order reassembly can already contain fragments beyond its current contiguous frontier. When a newly processed segment fills a gap, those previously retained fragments can immediately become contiguous as well. Advancing the frontier only to the end of the newly processed segment is therefore insufficient; the implementation must reconsider already-retained fragments that can now extend the contiguous region.

Merged master MR !11063, authored and merged by John Thacker, fixes TCP out-of-order reassembly in exactly this case. After a gap is filled, `msp_add_out_of_order()` now calls `find_maxnextseq()` so fragments already present in the multi-segment PDU can extend `maxnextseq`. The MR adds `challenge01_ooo_stream.pcapng.gz` and regression coverage that enables `tcp.reassemble_out_of_order`, checks that all expected HTTP responses are recovered, and runs the scenario both normally and with `tshark -2`. Release backport !11114, reviewed in an earlier batch, independently reinforces that the fix was considered release-worthy.

**Correctness rule:** treat the contiguous frontier as a derived property of the current fragment set, not merely as the end offset of the most recently processed segment. Whenever an operation can bridge a gap, re-evaluate retained fragments that may now be reachable before deciding whether later out-of-order data is still blocked.

**Testing rule:** exercise a capture where an out-of-order segment is retained, an earlier missing segment later arrives, and the already-retained data then exposes additional higher-layer PDUs. For stateful reassembly changes, include both first-pass and `-2` redissection coverage when the two execution models can differ.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker, with a dedicated reproducer and first-/second-pass regression tests, plus an accepted release backport.

## Cache the frontier, but invalidate it only when fragment topology can change it

A correct contiguous-frontier calculation can still become expensive if it repeatedly scans a long fragment chain inside another loop. Reassembly code should use the framework's known first gap as the natural starting point and avoid recomputing the frontier until a mutation can actually change it.

Merged master MR !11102, authored and merged by John Thacker, optimizes the !11063 path. `find_maxnextseq()` starts at `fd_head->first_gap` when available instead of walking from the beginning of the fragment list, and it stops as soon as the next fragment begins beyond the current contiguous length. The caller tracks whether `maxnextseq` is current and recomputes it only after processing a fragment can change the contiguous region. The MR explicitly notes that repeatedly checking the contiguous length can otherwise be linear on long reassemblies, which compounds when performed inside the out-of-order processing loop.

**Performance rule:** when a reassembly framework already tracks the first unresolved gap, use that as the lower bound for contiguous-frontier work. Do not rescan known-complete prefixes.

**Invalidation rule:** cache derived reassembly state only across operations that cannot change it. After inserting, consuming, or otherwise changing fragments near the frontier, invalidate/recompute before using the cached value for a control-flow decision.

**Review rule:** inspect nested loops in reassembly paths for repeated list/tree scans. A helper that is individually linear can become quadratic when called after every fragment; correctness-preserving incremental state or gap metadata is preferable when the fragment set can be large.

**Confidence:** Very high. Merged master performance/correctness refinement authored and merged by John Thacker immediately after the underlying out-of-order reassembly fix.
