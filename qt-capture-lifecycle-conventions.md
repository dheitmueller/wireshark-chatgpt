# Wireshark Qt Capture-File Lifecycle Conventions

## Separate closing teardown from closed UI state

Merged MR !1649 distinguishes `WiresharkDialog::captureFileClosing()` from `WiresharkDialog::captureFileClosed()` across many dialogs. The closing hook is for disconnecting tap listeners and teardown while the capture is closing. The closed hook is for widget state that depends on the capture file already being gone.

Merged MR !1622 corroborates the distinction: actions that require a live capture must be disabled after close to avoid stale state and crashes.

**Implementation rule:** release or disconnect capture-owned resources in the closing phase, then update controls and other post-close state in the closed phase.

**Architecture rule:** prefer the shared `WiresharkDialog` lifecycle hooks over per-dialog raw capture-event listeners when the behavior is fundamentally file-closing or file-closed behavior.

**Confidence:** High. The lifecycle split was merged across many dialogs and is corroborated by related close-state fixes.
