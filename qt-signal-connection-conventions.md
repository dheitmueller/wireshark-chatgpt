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

## Use queued delivery to break known unsafe synchronous Qt action paths, in both GUI frontends

A default direct Qt action connection can enter an unsafe synchronous call chain in affected framework versions. Merged master MR !9901 works around Qt issue QTBUG-106718 by connecting Export Packet Bytes and affected import/export actions using `Qt::QueuedConnection`. Tomasz Moń noted that the official Qt fix would arrive in later maintenance releases, but Linux distributions could retain vulnerable versions for some time. Gerald Combs and John Thacker explicitly requested the equivalent Logray fix alongside Wireshark's copy-pasted UI path; the merged changes include both.

**Implementation rule:** when a synchronous Qt signal path triggers a reproducible crash or reentrancy problem, a deliberate queued connection can be a compatibility workaround if receiver lifetime and event-loop behavior are understood. Describe which dependency versions need the workaround and test the corresponding Wireshark and Logray paths before merging. Do not assume the workaround replaces the upstream framework fix.

**Confidence:** Very high. Merged crash fix with direct Gerald Combs and John Thacker review, supported by the author's Qt-version rationale. Merged !9912 separately reinforces cross-frontend parity for selected-field menu state.

## Finish persistence before emitting a dialog's accepted outcome

**Merged master !9394**, authored by **John Thacker**, fixes ColoringRulesDialog: Qt Designer had a button-box `accepted()` → `accept()` connection while an auto-connected `on_buttonBox_accepted()` slot was also writing the rule file. The dialog could close and trigger packet-list recoloring before its new colors were saved. The fix removes the UI-file connection and lets the persistence slot call `done(QDialog::Accepted)` only on success, or `done(QDialog::Rejected)` after reporting failure. Accepted backports !9395 and !9396 corroborate the behavior.

**UI rule:** for dialogs whose acceptance triggers consumers of edited state, establish a single controlled accept path; perform validation/persistence first, then publish the accepted result. Audit both `.ui` signal connections and Qt auto-connected slot naming when diagnosing duplicate/out-of-order handlers. **Confidence:** very high, merged John Thacker fix and two release branches.

## Modernize Qt connections together with action ownership and frontend parity

Merged **!9008** replaces legacy Qt `SIGNAL`/`SLOT` string connections in multiple Qt widgets/models with typed member-function or lambda connections, including packet-list deferred work. Merged **!8996** explicitly connects Analyze-menu actions in both Wireshark and Logray rather than depending on Designer auto-connect names. **Martin Mathieson** later reported that the “Add as Column” context-menu action disappeared with a `QWidget::insertAction: Attempt to insert null action` warning after rebuilding.

**Rule:** a typed connection makes signature errors easier to detect, but changing action identifiers and moving connections out of generated `.ui` files requires an audit of every menu and dynamically populated context menu that reuses the action. Test newly launched GUI, selection-dependent menus, keyboard shortcuts, Logray parity and both Qt versions. For deferred lambdas, preserve receiver lifetime and avoid capturing stack references. A merged bulk conversion is not proof that follow-up action regressions were absent.

**Confidence:** High for accepted conversion style; very high that the post-merge null-action report warrants regression tests, without attributing all possible symptoms to one unseen cause.
