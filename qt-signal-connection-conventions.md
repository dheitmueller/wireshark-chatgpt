# Wireshark Qt Signal-Connection Conventions

This file records durable Qt signal/slot connection and disconnection conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Disconnect only the signal/slot relationship you own

A broad Qt disconnect can remove framework or component-internal connections in addition to the application connection that motivated the cleanup. In particular, wildcard forms that disconnect every signal from an object can also remove `QObject::destroyed` connections used by Qt or dependent objects to invalidate pointers safely.

Merged MR !21128 fixes a crash/warning path by replacing `disconnect(object, nullptr, nullptr, nullptr)` with a disconnect naming the exact signal and slot that Wireshark intended to remove. The MR explicitly notes that the wildcard form also disconnected `destroyed`, and newer Qt behavior exposed the resulting stale relationship. An earlier release-4.4 backport attempt, !21126, was closed and is therefore not treated as accepted implementation precedent; !21128 is the accepted fix.

**Implementation rule:** retain enough connection identity to disconnect the specific signal/receiver/slot or connection handle that application code owns. Do not use wildcard disconnect as a general cleanup shortcut when the sender or receiver may participate in other application, framework, or lifetime-management connections.

**Review implication:** when a Qt crash appears after object destruction or reconnection, inspect broad `disconnect()` calls as well as missing connections. A cleanup operation can be over-broad and silently remove the very destruction notification that makes another relationship safe.

**Confidence:** Very high. Merged correctness fix with a concrete failure mechanism; the superseded sibling was explicitly down-weighted.
