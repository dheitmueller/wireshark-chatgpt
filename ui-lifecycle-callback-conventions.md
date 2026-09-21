# Wireshark UI Lifecycle Callback Conventions

This file records durable conventions for ownership of UI lifecycle events and callbacks extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Let one layer own each lifecycle transition

Framework lifecycle hooks should have one authoritative trigger. A subclass or wrapper must not manually invoke begin/end callbacks when the underlying operation already emits the lifecycle event that causes the base class to invoke those callbacks.

Merged master MR !14745, authored and merged by John Thacker, removes explicit `beginRetapPackets()` and `endRetapPackets()` calls from `TapParameterDialog`. Subclasses that require a retap perform it through `fillTree()`, which calls `cf_retap_packets()`. That operation emits the Retap Started/Finished capture events, and the inherited `WiresharkDialog` event handling already calls the corresponding begin/end hooks. Calling them explicitly in `TapParameterDialog` therefore ran the lifecycle twice; `ServiceResponseTables` exposed the bug by adding each table twice.

**Architecture rule:** identify which layer owns the lifecycle transition and invoke the operation, not its downstream lifecycle callbacks. If a framework operation emits start/finish events consumed by a base class, derived classes should not duplicate those hooks around the same operation.

**Review rule:** when begin/end, start/finish, reset/finalize, or similar hooks appear in both a wrapper and a framework/event path, trace the full call/event chain before adding another explicit invocation. Duplicate lifecycle delivery may be hidden for simple views yet corrupt state in multi-instance or multi-table views.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker with a concrete duplicate-state symptom and an explicit explanation of the event path.

## Initialize action state from current backing-object validity, not only later lifecycle signals

An observer can be created while the object it observes is already absent or invalid. In that case no future "closed" or "became invalid" transition is guaranteed to arrive, so enabling actions solely from transition callbacks leaves the UI in a state that was never valid.

Merged master MR !14705, authored and merged by John Thacker, fixes `TapParameterDialog` when it is opened without a usable capture file. Existing code disabled Edit/Apply after receiving the capture-file-closed event, but a dialog created with no capture file never received such a transition; pressing Apply could produce heap-use-after-free behavior and garbage tap-table entries. The accepted code calls `updateWidgets()` during construction and disables the actions whenever `file_closed_ || !cap_file_.isValid()`.

**Architecture rule:** initialize UI capability state by querying the current validity of the backing model/resource, then keep it synchronized with lifecycle events. Transition notifications are for changes after observation begins; they are not a substitute for establishing the initial state.

**Review rule:** test dialogs and actions created in every relevant initial lifecycle state—not only the normal "resource exists, then closes" path. In particular, construct the view with no capture, an already-closed capture, and a valid capture, and verify that destructive/expensive actions are enabled only when their backing object can satisfy them.

**Confidence:** Very high. Merged master memory-safety/UI-state fix authored and merged by John Thacker with the missing-initial-event failure mode documented explicitly.
