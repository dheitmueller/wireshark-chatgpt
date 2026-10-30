# Wireshark Qt Object Parenting Conventions

This file records durable Qt object-parenting and ownership conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## QObject parentage does not imply QWidget parentage

Qt's generic ownership hierarchy is broader than the widget hierarchy. A `QAction` or other `QObject` may legitimately be parented to a non-widget application object, so code that later requires a `QWidget` parent must not assume that `parent()` can safely be downcast to one.

Merged MR !25271, authored and merged by John Thacker, fixes `FunnelConsoleAction::triggerCallback()`. `FunnelConsoleAction` is a `QAction` whose parent is `mainApp`, while `IOConsoleDialog` requires a `QWidget`. The old code used `qobject_cast<QWidget *>(parent())` and immediately dereferenced the result into a reference argument. When the cast returned null, UBSan reported reference binding to a null pointer. The accepted fix obtains the actual `MainWindow` from `mainApp->mainWindow()`, checks it for null, and only then constructs the dialog.

**Implementation rule:** when an API requires a `QWidget` owner, obtain a semantically guaranteed widget owner rather than deriving one from generic `QObject` parentage. Treat `qobject_cast` failure as a normal possibility and never dereference or bind a reference from its result without establishing non-nullness.

**Testing rule:** sanitizer findings in GUI paths are actionable even when the UI appears to work normally. Undefined reference binding or invalid ownership assumptions may remain latent until a particular startup/lifecycle state occurs.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker with a concrete UBSan diagnostic and a narrowly targeted ownership correction.


## Parent auxiliary graph QObjects to their actual visual owner

Merged master MR !9923, authored by John Thacker, changes `RtpAudioGraph` construction to `QObject(audio_plot)`, parenting the helper object to its `QCustomPlot` owner. This ensures that graph helper lifetime follows the plot's lifecycle rather than leaving an auxiliary QObject without the intended owner.

**Implementation rule:** for QObject helpers whose semantic lifetime is strictly subordinate to a plot, dialog, or other QObject, establish Qt parentage explicitly when creating the helper. Keep ownership consistent with any manual deletion paths, and do not assume C++ pointer membership alone communicates destruction responsibility.

**Confidence:** High. Merged maintainer-authored leak/lifecycle correction; use current Qt code as authority for the precise class relationship.

## Parent button-owned QActions to the buttons that expose them

A dynamically allocated `QAction` does not automatically become owned by a `QToolButton` just because it is installed as the default action. Merged master MR !9902 fixes leaks in RTP Analysis and RTP Player windows by constructing each dynamically created action with the associated tool button as its QObject parent.

**Implementation rule:** when a dynamically constructed action exists solely to serve a button or menu, give it an explicit QObject parent whose lifetime dominates that action. Audit copied GUI helpers for parentless actions, and prefer ownership by the actual UI component rather than manual cleanups in unrelated window code.

**Confidence:** High. Merged master lifecycle fix; corroborates the established Qt-parentage conventions.
