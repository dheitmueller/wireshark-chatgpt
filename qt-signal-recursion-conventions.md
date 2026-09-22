# Wireshark Qt Signal Recursion Conventions

This file records durable conventions for preventing unintended signal/slot feedback loops in Wireshark's Qt UI. Current upstream Qt code remains authoritative.

## Guard programmatic model/view updates that synchronously invoke their own change handler

Qt model/view setters can emit signals synchronously. If a signal handler responds by calling the same update routine that issued the setter, a programmatic reconciliation can become an unbounded feedback loop rather than a one-way state update.

Merged master MR !13365, authored and merged by John Thacker, fixes a stack-smashing recursion in `CaptureOptionsDialog`. `InterfaceTreeWidgetItem::updateInterfaceColumns()` could call `setData()`, which emitted `dataChanged`, leading through `itemChanged` and `interfaceItemChanged` back into `updateInterfaceColumns()`. The loop was rare because it required an interface whose monitor-mode capability changed when Wireshark tried to apply the setting, but when triggered it recursed indefinitely. The accepted fix blocks the tree widget's signals while the internal column reconciliation runs. Release backports !13366, !13367, and !13368 carry the same fix to 4.2, 4.0, and 3.6 respectively.

**Implementation rule:** distinguish user-originated changes from internal state reconciliation. When an internal model/view update would synchronously emit the signal whose handler invokes that same update path, suppress or guard that feedback signal for the duration of the programmatic mutation, then restore normal signaling immediately afterward.

**Lifecycle rule:** any signal-suppression guard must restore the prior signaling state on every exit path. Keep the guarded region as small as practical so unrelated UI notifications are not accidentally dropped.

**Review rule:** when a Qt handler mutates the object that emitted the signal, trace the synchronous signal chain before assuming the update is idempotent. A path such as setter → `dataChanged`/`itemChanged` → handler → same setter is a recursion hazard even if it appears only when hardware capability or model state changes underneath the UI.

**Testing rule:** exercise the exceptional state transition that forces the programmatic correction, not only the stable/common case. For capture-interface UI, include a device that initially advertises a capability but rejects or loses it when the option is applied, and verify that the UI converges without recursion while subsequent genuine user changes still emit normally.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker and independently carried to three supported release branches.