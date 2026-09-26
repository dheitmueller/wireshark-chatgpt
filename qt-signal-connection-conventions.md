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
