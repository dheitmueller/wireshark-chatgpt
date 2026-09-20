# Wireshark Live-Capture Retap Conventions

This file records durable conventions for retapping or rescanning packets while a live capture can continue to grow. Current upstream source remains authoritative.

## Retap a stable frame snapshot when the live capture can grow concurrently

A retap that walks the capture while new live packets can still be appended must define which frames belong to that traversal. Without a stable upper bound, a frame arriving during the retap can be processed once by the retap and then again by the normal live-tail path, duplicating tap statistics or other observer effects.

Merged master MR !15166, authored and merged by John Thacker, fixes duplicate tapping during live capture by snapshotting the current frame count before `process_specified_records()` and limiting the retap to frames that existed at that boundary. The new comments explicitly distinguish offline reads, where `read_lock` already prevents this overlap, from live-tail processing, where packets legitimately continue to arrive.

**Implementation rule:** when traversing a mutable capture for a logically point-in-time operation, snapshot the traversal boundary before iteration and do not silently absorb objects appended after that boundary unless the operation's contract explicitly calls for a live view.

## Validate exclusivity before resetting observer state

A retap commonly resets tap listeners/statistics before replaying packets. If the replay cannot start because another read/retap is already active, that reset must not happen first; otherwise the in-progress operation continues against state that was partially cleared underneath it.

The same merged !15166 moves the `read_lock` rejection ahead of listener reset and retap-start notification. John Thacker's rationale notes that the old order could leave tap statistics invalid even though the nested traversal itself was rejected.

**Implementation rule:** perform lock/reentrancy/admission checks before any destructive preparation for a traversal. Once observer state is reset, the code should already know that the corresponding replay can proceed, or it must have a transactional rollback path.

**Review rule:** for retap/rescan changes, reason about three timelines together: the frame set being traversed, packets that may arrive concurrently, and tap/listener state being reset or rebuilt. Tests should include an active live capture, not only a completed offline file.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker, addressing both duplicate live-capture processing and partial listener-state invalidation.
