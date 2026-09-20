# Wireshark UI State Stack Conventions

This file records durable conventions for stacked or temporary UI state. Current upstream source remains authoritative.

## Push and pop all attributes that belong to one visible state together

When a widget maintains a stack of temporary states, every presentation attribute whose meaning belongs to that state must travel with the stack entry. Mutating associated metadata separately from the stacked primary value causes the metadata to be lost or mismatched when a temporary state is pushed and later popped.

Merged master MR !15835, authored and merged by John Thacker, fixes `LabelStack` by adding the tooltip to each `StackItem` alongside its text and context. Previously the status text was pushed and restored, while `MainStatusBar` set or cleared the tooltip directly on the widget. A temporary status such as field information could therefore overwrite the tooltip for an underlying file-status entry; popping the temporary status restored the text but not the corresponding tooltip. The accepted implementation pushes and pops the text and tooltip as one state object.

**Implementation rule:** if a stack, history, or temporary-context mechanism represents one logical UI state, store all state-coupled attributes in the stack entry and restore them atomically. Do not keep one attribute in the stack while updating a dependent attribute directly on the live widget.

**Review rule:** exercise nested temporary-state transitions, not only the steady state. Verify that after push/pop the complete underlying presentation is restored, including secondary attributes such as tooltips, styling, icons, or accessibility metadata when they belong to the same logical state.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker, with the lost-tooltip push/pop failure mode stated explicitly in the commit.

## Drive stacked UI state from the lifecycle event that actually owns the transition

When an operation emits lifecycle callbacks, update the visible state in the callback that represents the real transition rather than immediately before or after the function that happened to initiate it. This matters when one operation can produce repeated transitions, such as capture file rotation in multiple-file mode.

Merged master MR !15777 moves the live-capture status push into the `CapturePrepared` and capture-update-started event handlers. `capture_start()` itself already triggers `CapturePrepared`, and multiple-file capture can later switch files without returning through the original call site. Updating the status after `capture_start()` therefore modeled the initiating call rather than the actual capture/file lifecycle and left the stack incorrect after rotation.

**Implementation rule:** attach push/pop operations to the event boundary that defines the state, including repeated instances of that event. Avoid duplicating a state transition around both an initiating API call and the callback it triggers.

**Review rule:** for event-driven UI state, test repeated transitions such as capture restart/file rotation, not just the first start/stop pair. Verify that every push has a lifecycle-matched replacement or pop and that indirect callbacks do not create duplicate entries.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker with the lifecycle mismatch described directly in the MR.