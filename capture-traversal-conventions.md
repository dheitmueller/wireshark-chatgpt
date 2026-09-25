# Wireshark Capture-Traversal Conventions

This file records durable conventions for analyses that iterate an existing capture file. Current upstream source remains authoritative.

## Whole-capture analyses must participate in capture-file read exclusion

A statistics or analysis routine that walks stored frames is not isolated from ordinary capture-file lifecycle operations merely because it does not modify packet bytes. Closing a file, rescanning it, or starting another traversal while that routine is reading frame data can invalidate the objects it is using and can also corrupt shared progress/stop UI state.

Merged master MR !9452, authored and merged by John Thacker, fixes Protocol Hierarchy Statistics so it first rejects entry when `capture_file::read_lock` is already held, then acquires that lock for the duration of the traversal. The UI action is disabled while a read/rescan is in progress, and the implementation uses the capture file's shared `stop_flag` rather than a private local boolean so the common Stop control actually stops the operation. The lock is released on both normal completion and the path that discards partial statistics after cancellation.

**Implementation rule:** any operation that iterates the capture's stored records/frames and is unsafe against file close, rescan, or another whole-file traversal should participate in the capture-file read-lock/admission contract. Perform the admission check before traversal-side effects, acquire the lock before dereferencing traversal state, and release it on every exit path.

**UI rule:** if an operation uses Wireshark's shared progress/stop machinery, use the shared cancellation state expected by that machinery rather than an unrelated local flag. Disable commands that cannot safely start while a conflicting read/rescan owns the capture.

**Review rule:** for a new whole-capture statistic or batch analysis, inspect not only the packet loop but also file-close behavior, rescans/retaps, progress-dialog ownership, cancellation, and every early-return cleanup path.

**Confidence:** Extremely high. Merged master correctness fix authored and merged by John Thacker, addressing concrete crashes and progress/Stop-button anomalies.
