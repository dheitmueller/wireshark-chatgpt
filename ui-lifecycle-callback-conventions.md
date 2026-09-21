# Wireshark UI Lifecycle Callback Conventions

This file records durable conventions for ownership of UI lifecycle events and callbacks extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Let one layer own each lifecycle transition

Framework lifecycle hooks should have one authoritative trigger. A subclass or wrapper must not manually invoke begin/end callbacks when the underlying operation already emits the lifecycle event that causes the base class to invoke those callbacks.

Merged master MR !14745, authored and merged by John Thacker, removes explicit `beginRetapPackets()` and `endRetapPackets()` calls from `TapParameterDialog`. Subclasses that require a retap perform it through `fillTree()`, which calls `cf_retap_packets()`. That operation emits the Retap Started/Finished capture events, and the inherited `WiresharkDialog` event handling already calls the corresponding begin/end hooks. Calling them explicitly in `TapParameterDialog` therefore ran the lifecycle twice; `ServiceResponseTables` exposed the bug by adding each table twice.

**Architecture rule:** identify which layer owns the lifecycle transition and invoke the operation, not its downstream lifecycle callbacks. If a framework operation emits start/finish events consumed by a base class, derived classes should not duplicate those hooks around the same operation.

**Review rule:** when begin/end, start/finish, reset/finalize, or similar hooks appear in both a wrapper and a framework/event path, trace the full call/event chain before adding another explicit invocation. Duplicate lifecycle delivery may be hidden for simple views yet corrupt state in multi-instance or multi-table views.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker with a concrete duplicate-state symptom and an explicit explanation of the event path.