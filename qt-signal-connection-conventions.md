# Wireshark Qt Signal-Connection Conventions

This file records durable Qt signal/slot and action-wiring conventions extracted from accepted Wireshark changes. Current upstream source and Qt documentation remain authoritative.

## Prefer explicit typed connections over connection-by-name conventions

Use explicit typed `connect()` calls for menu actions and other important UI wiring instead of relying on `on_action..._triggered` naming conventions or string-based signal/slot signatures. Explicit connections make the relationship visible at the call site and allow the compiler to validate signal/slot types.

Merged master MRs !8171, !8196, !8197, and !8209, largely authored by Gerald Combs, progressively migrate Edit/File/menu action wiring to this pattern. The changes also rename helper methods away from names that imply Qt auto-connection magic when they are ordinary methods.

**Implementation rule:** for new or touched Qt action wiring, prefer `connect(sender, &Type::signal, receiver, ...)` (including a small lambda when needed) over connection-by-name or `SIGNAL()/SLOT()` strings.

## Choose direct vs queued delivery based on event/object lifetime

A signal handler that can close/destroy the menu that emitted the action, enter a nested event loop, or otherwise invalidate objects still participating in the current dispatch may need queued delivery.

Merged master MR !8171 explicitly uses `Qt::QueuedConnection` for mark/ignore/time-reference/time-shift/preferences actions used in packet-list/detail context menus. The code comment states that queued connections prevent the context menus from being destroyed prematurely, and a reporter confirmed the associated crash was no longer reproducible.

**Implementation rule:** do not treat `Qt::QueuedConnection` as a style preference. Use it when handler side effects must be deferred until the current event/signal stack unwinds; otherwise prefer the normal typed connection behavior.

**Review rule:** when changing menu/action wiring, review connection type together with object lifetime and nested event processing. A migration that changes connection syntax but silently changes delivery timing can reintroduce lifecycle bugs.

**Confidence:** Very high. Merged Gerald Combs changes with concrete crash reproduction/verification and repeated follow-up migration across menu families.


## Queue menu actions across nested event-loop lifetime hazards

A deferred-delete API is not enough by itself when the action handler can enter a nested Qt event loop. `WA_DeleteOnClose` ultimately uses `deleteLater()`, but a nested `processEvents()` can process that pending deletion before the current QAction/QMenu dispatch stack returns.

Closed MR 8081 records the failed design exploration; Tomasz Mon identified the nested-event-loop ordering problem and recommended the accepted MR 8088 solution: connect the affected actions using `Qt::QueuedConnection`. The queued handler runs after menu handling has unwound, avoiding use-after-destruction. MR 8109 carries the same fix to release-4.0.

**Implementation rule:** when a slot can re-enter the event loop or cause sender-owned UI state to be destroyed while the current signal stack still depends on it, use queued delivery to create an explicit asynchronous lifetime boundary. Do not treat `deleteLater()` as proof that deletion cannot happen during nested event processing.

**Confidence:** Extremely high. The accepted master fix is preceded by detailed failure analysis and followed by a stable backport.


## Nested event loops can invalidate deferred-deletion assumptions

Merged master MR !8038, authored by John Thacker, fixes a Qt 6.3 ExportDissectionDialog lifetime failure by moving export work to the earlier `filesSelected` signal. Guy Harris questioned whether that signal ordering is guaranteed across Qt releases. Tomasz Moń identified Wireshark's nested `MainApplication::processEvents()` call as the mechanism that let a pending `DeferredDelete` run before the original handler returned, and described eliminating unnecessary nested event loops as the long-term fix. John agreed that the earlier signal is a practical workaround rather than a permanent ordering guarantee. Release-4.0 MR !8054 carries the same fix.

**Architecture rule:** `deleteLater()` does not guarantee that an object survives until the current slot returns if that slot enters a nested event loop. Prefer designs that avoid explicit nested `processEvents()` or `exec()` loops; if re-entry is unavoidable, make ownership and scheduling boundaries explicit.

**Review rule:** a timing fix that moves work to an earlier signal is only as strong as the API's documented signal/lifetime guarantees. Treat incidental ordering as a workaround, not an architectural invariant.

**Confidence:** Extremely high. Merged John Thacker fix with direct Guy Harris review and Tomasz Moń root-cause analysis.


## Typed connections catch signature mistakes during ordinary cleanup

Merged master MR !7224, authored by Gerald Combs, fixes an incorrect QComboBox signal assumption and removes a duplicate connection while converting touched RTP Player wiring from string-based `SIGNAL()/SLOT()` connections to typed member-pointer connections.

**Implementation rule:** when touching Qt signal wiring, prefer typed `connect(sender, &Type::signal, receiver, &Type::slot)` forms. The compiler can then reject mismatched signal/slot signatures that old string-based connections may hide.

**Confidence:** Very high. Merged master change authored by Gerald Combs.
