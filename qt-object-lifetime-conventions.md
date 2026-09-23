# Wireshark Qt Object-Lifetime Conventions

This file records durable Qt object lifetime and deletion conventions extracted from accepted upstream Wireshark changes. Current upstream source and Qt documentation remain authoritative.

## Use QObject parent ownership for child objects that should live exactly as long as a widget

Heap-allocated QObjects such as actions should normally be given the widget/object that semantically owns them as their QObject parent. Relying on an unparented object merely because another Qt API references it can leak the object when the dialog is destroyed.

Merged master MR !11813, authored and merged by João Valverde, fixes the Manuf dialog by constructing its `QAction` instances with `ui->manufTableView` as parent. Adding an action to the widget's action list did not by itself establish the ownership needed to delete the action; explicit QObject parentage does.

**Implementation rule:** when a dynamically allocated QObject has the same lifetime as a dialog/widget, express that lifetime with QObject parentage at construction whenever possible. A signal connection, model entry, action registration, or other reference is not automatically an ownership relationship.

**Confidence:** Very high. Merged master leak fix authored and merged by João Valverde.

## Defer deletion when an object can still participate in the current Qt event dispatch

Deleting a QObject as a side effect of the event that is currently using it can invalidate state still needed by Qt or by queued/direct signal delivery. When the object should disappear after the current event completes, prefer Qt's deferred-deletion mechanism rather than immediate destruction triggered by a convenience attribute.

Merged master MR !11816, authored and merged by Gerald Combs, fixes a crash in the capture-interface context menu by replacing `Qt::WA_DeleteOnClose` with a `QMenu::triggered` connection to `deleteLater()`. The MR identifies QTBUG-106718 and notes that `WA_DeleteOnClose` combined with the queued connection path was unsafe here. Release-4.0 backport !11817 carries the same fix.

**Implementation rule:** for transient menus/dialogs involved in signal dispatch, choose deletion timing deliberately. If immediate destruction can race the active event/signal stack, schedule `deleteLater()` so teardown occurs after control returns to the event loop.

**Review rule:** lifetime bugs around menus and dialogs should be reviewed together with signal connection type and nested/queued event processing. "Close" and "destroy now" are not interchangeable operations in event-driven code.

**Confidence:** Very high. Merged master crash fix from Gerald Combs with an accepted stable backport and a documented upstream Qt bug interaction.

## Put final cleanup in the object's destructor when cleanup must occur for every close path

Cleanup tied only to an accepted/button-handler path is incomplete when Qt can destroy the dialog through other close paths, and it can also run too early when closing triggers nested event processing before the object is actually destroyed.

Merged master MR !11857, authored and merged by Gerald Combs, moves Manage Interfaces dialog cleanup from `on_buttonBox_accepted()` into the destructor. The previous cleanup could interact badly with `WA_DeleteOnClose` and nested event loops, leaving later callbacks with state that had already been torn down. The destructor establishes one final cleanup boundary independent of how the dialog is closed.

**Implementation rule:** if resource/state cleanup is an invariant of object destruction rather than a semantic consequence of one particular button, place it in the destructor (or an equivalent single ownership teardown point). Do not pre-destroy shared state in an acceptance handler merely because that is the most common close path.

**Review rule:** enumerate all ways a Qt object can close or be destroyed and consider nested event loops that may run between a close request and actual destruction. Cleanup should occur at the ownership boundary that makes those paths equivalent.

**Confidence:** Very high. Merged master lifetime fix authored and merged by Gerald Combs, with concrete reproduction/testing and explicit discussion of the nested-event-loop hazard.
