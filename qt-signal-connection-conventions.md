# Wireshark Qt Signal-Connection Conventions

This file records durable Qt signal/slot connection and disconnection conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Disconnect only the signal/slot relationship you own

A broad Qt disconnect can remove framework or component-internal connections in addition to the application connection that motivated the cleanup. In particular, wildcard forms that disconnect every signal from an object can also remove `QObject::destroyed` connections used by Qt or dependent objects to invalidate pointers safely.

Merged MR !21128 fixes a crash/warning path by replacing `disconnect(object, nullptr, nullptr, nullptr)` with a disconnect naming the exact signal and slot that Wireshark intended to remove. The MR explicitly notes that the wildcard form also disconnected `destroyed`, and newer Qt behavior exposed the resulting stale relationship. An earlier release-4.4 backport attempt, !21126, was closed and is therefore not treated as accepted implementation precedent; !21128 is the accepted fix.

**Implementation rule:** retain enough connection identity to disconnect the specific signal/receiver/slot or connection handle that application code owns. Do not use wildcard disconnect as a general cleanup shortcut when the sender or receiver may participate in other application, framework, or lifetime-management connections.

**Review implication:** when a Qt crash appears after object destruction or reconnection, inspect broad `disconnect()` calls as well as missing connections. A cleanup operation can be over-broad and silently remove the very destruction notification that makes another relationship safe.

**Confidence:** Very high. Merged correctness fix with a concrete failure mechanism; the superseded sibling was explicitly down-weighted.

## Re-audit AutoConnection semantics when event-loop or lifetime structure changes

`Qt::AutoConnection` is not a fixed delivery guarantee. Refactoring a dialog from a nested `exec()` event loop to non-blocking lifetime management can turn a previously deferred-looking interaction into a synchronous direct call, changing reentrancy and destruction behavior even if the signal/slot declaration itself is untouched.

Merged release MRs !15210 and !15211, authored and merged by John Thacker, carry the accepted Time Shift crash fix. After the nested dialog event loop was removed, the `timeShifted` connection became a direct call into `PacketList::applyTimeShift()`. Redissection could run while the `WA_DeleteOnClose` dialog was closed and destroyed; when control eventually returned, the dialog attempted to update widgets that no longer existed. The fix makes the connection explicitly `Qt::QueuedConnection`, breaking that synchronous call stack and allowing the emitting operation to complete without later touching a deleted dialog.

**Implementation rule:** after changing modal/nested event loops, thread affinity, `DeleteOnClose`, or other object-lifetime structure, re-evaluate every nearby `AutoConnection` whose slot can perform long-running, reentrant, or event-processing work. If correctness requires deferred delivery, state that requirement explicitly with `Qt::QueuedConnection` instead of relying on the old event-loop shape to provide it accidentally.

**Review implication:** for crash reports involving a dialog closed while redissection, retap, export, or another long operation is active, inspect both object ownership and signal delivery mode. A lifetime-safe design needs the connection type and the object's destruction policy to agree.

**Confidence:** Very high. The accepted fix was authored by John Thacker and carried into both maintained release branches; the failure mechanism is explicitly documented in the commit/MR description.