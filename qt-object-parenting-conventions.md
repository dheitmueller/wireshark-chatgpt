# Wireshark Qt Object Parenting Conventions

This file records durable Qt object-parenting and ownership conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## QObject parentage does not imply QWidget parentage

Qt's generic ownership hierarchy is broader than the widget hierarchy. A `QAction` or other `QObject` may legitimately be parented to a non-widget application object, so code that later requires a `QWidget` parent must not assume that `parent()` can safely be downcast to one.

Merged MR !25271, authored and merged by John Thacker, fixes `FunnelConsoleAction::triggerCallback()`. `FunnelConsoleAction` is a `QAction` whose parent is `mainApp`, while `IOConsoleDialog` requires a `QWidget`. The old code used `qobject_cast<QWidget *>(parent())` and immediately dereferenced the result into a reference argument. When the cast returned null, UBSan reported reference binding to a null pointer. The accepted fix obtains the actual `MainWindow` from `mainApp->mainWindow()`, checks it for null, and only then constructs the dialog.

**Implementation rule:** when an API requires a `QWidget` owner, obtain a semantically guaranteed widget owner rather than deriving one from generic `QObject` parentage. Treat `qobject_cast` failure as a normal possibility and never dereference or bind a reference from its result without establishing non-nullness.

**Testing rule:** sanitizer findings in GUI paths are actionable even when the UI appears to work normally. Undefined reference binding or invalid ownership assumptions may remain latent until a particular startup/lifecycle state occurs.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker with a concrete UBSan diagnostic and a narrowly targeted ownership correction.
