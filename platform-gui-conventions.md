# Wireshark Platform and GUI Conventions

This file records durable platform-support and GUI/concurrency conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Platform minimums are product/distribution policy, not only build settings

Raising a minimum supported operating-system version can be the cleanest architectural way to remove compatibility shims, but it changes the contract with users and distribution infrastructure. Treat it as a project-wide compatibility decision requiring explicit consensus and coordinated update/distribution behavior.

Merged MR !25993, authored by John Thacker and merged by Anders Broman, raises Wireshark's Windows minimum from Windows 10 1809 / Server 2019 to Windows 10 21H2 / Server 2022 so the application can rely on Windows support for forcing the active code page to UTF-8. That platform guarantee permits later simplification of CLI, filesystem, and Lua Unicode workarounds. John explicitly marked the otherwise technically ready MR as draft until project consensus was established.

The MR also notes that the website/updater must be changed so unsupported Windows versions are not offered an incompatible Wireshark release, while those systems can remain on the maintained 4.6.x line.

**Architecture rule:** when a platform baseline eliminates compatibility layers, evaluate the simplification against the support cost at the product level. Coordinate installer/updater/download gating and documentation, and identify the supported prior release for users left on the older platform.

**Submission/review rule:** a technically correct platform-minimum change is not merge-ready until support-policy consensus exists. Draft status is appropriate while that policy decision is unresolved.

**Confidence:** Very high. Merged platform-policy change authored by John Thacker, held explicitly for consensus, and merged by Anders Broman.

## External callbacks must respect Qt object thread affinity

QObject-derived state must be manipulated on the thread that owns the object when the Qt API requires it. An external/native library callback does not inherit that affinity merely because it calls a method on a Qt-facing class.

Merged MR !25996, authored and merged by John Thacker, fixes Windows software-update handling because WinSparkle can call `SoftwareUpdate::softwareUpdateEngaged()` from another thread while the method starts/stops a `QTimer`. Since a `QTimer` may only be started/stopped from its owning thread, the fix uses `QMetaObject::invokeMethod(..., Qt::QueuedConnection)` to enqueue the operation on the correct Qt thread.

**Implementation rule:** at boundaries from third-party/native callback threads into Qt UI/application objects, identify the owning thread before touching QObjects, timers, or UI state. Marshal work through a queued Qt invocation/signal when affinity is not guaranteed; do not rely on the callback's current thread.

**Confidence:** Very high. Merged correctness fix authored and merged by John Thacker with an explicit Qt thread-affinity rationale.
