# Wireshark Qt Long-Operation Lifetime Conventions

This file records durable conventions for Qt dialogs that initiate long-running Wireshark operations. Current upstream source remains authoritative.

## Once a dialog commits a long operation, prevent stale controls from destroying its live state

A dialog that starts an export, redissection, or similar long-running operation can remain logically involved after the user's initial acceptance. If the same dialog still exposes a generic close/cancel path while the operation is running, that control can destroy objects the operation still expects without actually cancelling the operation itself.

Merged master MR !15198, authored by John Thacker and merged by Anders Broman, fixes the Export Dissections dialog by hiding it as soon as acceptance starts the export. The operation's real cancellation mechanism is the progress dialog owned by the main window; leaving the accepted dialog visible let users click its Cancel button, which did not cancel the export but did delete the dialog, causing a crash when the export completed. Merged backports !15212 and !15213 carry the same behavior to maintained release branches.

**Implementation rule:** after a dialog hands control to a long-running operation, keep only controls whose semantics are still valid. If cancellation has moved to a separate progress/controller object, hide or disable the initiating dialog rather than leaving a misleading control that only tears down UI state.

**Lifetime rule:** the object that remains referenced by completion/progress callbacks must outlive those callbacks, or the callbacks must be disconnected/cancelled before destruction. UI visibility and close policy are part of this ownership contract, not merely presentation details.

**Review implication:** test accepting a long operation and then attempting to close/cancel every still-visible related window before completion, including cancellation through the intended progress UI. Verify that each control either genuinely cancels the operation or cannot destroy state still needed by it.

**Confidence:** Very high. Merged master correctness fix authored by John Thacker plus two merged release backports, with the crash mechanism and intended cancellation ownership explicitly documented.