# Wireshark Qt Long-Operation Lifetime Conventions

This file records durable conventions for Qt dialogs that initiate long-running Wireshark operations. Current upstream source remains authoritative.

## Once a dialog commits a long operation, prevent stale controls from destroying its live state

A dialog that starts an export, redissection, or similar long-running operation can remain logically involved after the user's initial acceptance. If the same dialog still exposes a generic close/cancel path while the operation is running, that control can destroy objects the operation still expects without actually cancelling the operation itself.

Merged master MR !15198, authored by John Thacker and merged by Anders Broman, fixes the Export Dissections dialog by hiding it as soon as acceptance starts the export. The operation's real cancellation mechanism is the progress dialog owned by the main window; leaving the accepted dialog visible let users click its Cancel button, which did not cancel the export but did delete the dialog, causing a crash when the export completed. Merged backports !15212 and !15213 carry the same behavior to maintained release branches.

**Implementation rule:** after a dialog hands control to a long-running operation, keep only controls whose semantics are still valid. If cancellation has moved to a separate progress/controller object, hide or disable the initiating dialog rather than leaving a misleading control that only tears down UI state.

**Lifetime rule:** the object that remains referenced by completion/progress callbacks must outlive those callbacks, or the callbacks must be disconnected/cancelled before destruction. UI visibility and close policy are part of this ownership contract, not merely presentation details.

**Review implication:** test accepting a long operation and then attempting to close/cancel every still-visible related window before completion, including cancellation through the intended progress UI. Verify that each control either genuinely cancels the operation or cannot destroy state still needed by it.

**Confidence:** Very high. Merged master correctness fix authored by John Thacker plus two merged release backports, with the crash mechanism and intended cancellation ownership explicitly documented.

## Serialize capture-file readers and share cancellation with the operation that owns the progress UI

Merged master !9452, authored by John Thacker, adds a `capture_file::read_lock` guard to Protocol Hierarchy statistics, disables the menu action while capture reads/rescans are underway, and binds its progress dialog to `cf->stop_flag` so the Stop button actually terminates that statistics pass. It releases the lock on completion. Previously the scan could race with file closing or a rescan, segfault, and overwrite an unrelated progress bar's state.

**Implementation rule:** when a user-triggered scan walks capture frames, serialize it against capture close/read/rescan and ensure every exit releases ownership. Its cancellation flag must be the same flag the progress UI controls. Disable entry points while the operation is invalid; do not merely disable redraws after a conflicting scan has begun.

**Regression rule:** attempt capture close, rescan, filter changes and Stop-button cancellation during long statistics work; verify the original operation's progress and lock are restored. **Confidence:** very high, merged John Thacker correctness fix.

## Cancelable packet-list sorting must share the capture-file read lock and rescan lifecycle

Merged master !9349, authored by John Thacker, adds cancellation to packet-list sorting while comparisons may dissect/parse column values. Sorting checks the capture read lock, shares an explicit stop flag with progress UI, and can be aborted without leaving an invalid sort state. The capture read lock is released before post-read callbacks, and sorting after captureFileReadFinished sees columns invalidated by the completed scan. Related merged !9343 removes an extra resetColumns() in filterPackets because captureFileReadFinished already performs the invalidation after a rescan.

**Rule:** do not perform reentrant dissection-backed sorting while the capture is being read/rescanned; test cancellation and guard every lock/progress exit. Once a canonical completion callback owns column invalidation, avoid duplicate resets in callers. Test filtering, closing, rescan, sort cancellation and subsequent sort order. **Confidence:** very high (two merged John Thacker correctness changes).
