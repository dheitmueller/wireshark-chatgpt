# Wireshark Platform and GUI Conventions

This file records durable platform-support and GUI/concurrency conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Platform minimums are product/distribution policy, not only build settings

Raising a minimum supported operating-system version can be the cleanest architectural way to remove compatibility shims, but it changes the contract with users and distribution infrastructure. Treat it as a project-wide compatibility decision requiring explicit consensus and coordinated update/distribution behavior.

Merged MR !25993, authored by John Thacker and merged by Anders Broman, raises Wireshark's Windows minimum from Windows 10 1809 / Server 2019 to Windows 10 21H2 / Server 2022 so the application can rely on Windows support for forcing the active code page to UTF-8. That platform guarantee permits later simplification of CLI, filesystem, and Lua Unicode workarounds. John explicitly marked the otherwise technically ready MR as draft until project consensus was established.

The MR also notes that the website/updater must be changed so unsupported Windows versions are not offered an incompatible Wireshark release, while those systems can remain on the maintained 4.6.x line.

Merged MR !26169 is the corresponding implementation evidence for that policy decision. Authored by John Thacker and approved/merged by Gerald Combs, it removes the now-redundant `cli_main` Windows command-line compatibility layer once the application manifest and supported-Windows baseline guarantee UTF-8 as the active code page. The MR also adds tests that would fail without the manifest/platform guarantee, so the simplification is protected by executable checks rather than only an assumption in build logic.

**Architecture rule:** when a platform baseline eliminates compatibility layers, evaluate the simplification against the support cost at the product level. Coordinate installer/updater/download gating and documentation, identify the supported prior release for users left on the older platform, and then remove compatibility shims whose contracts are genuinely subsumed by the new baseline rather than keeping parallel paths indefinitely.

**Testing rule:** when deleting a platform compatibility shim because a new minimum-platform guarantee replaces it, retain or add tests that exercise the formerly shimmed behavior so the external platform/manifest assumption remains observable.

**Submission/review rule:** a technically correct platform-minimum change is not merge-ready until support-policy consensus exists. Draft status is appropriate while that policy decision is unresolved.

**Confidence:** Very high. Merged platform-policy change authored by John Thacker and held explicitly for consensus, followed by a merged implementation simplification authored by John and approved/merged by Gerald Combs.

## External callbacks must respect Qt object thread affinity

QObject-derived state must be manipulated on the thread that owns the object when the Qt API requires it. An external/native library callback does not inherit that affinity merely because it calls a method on a Qt-facing class.

Merged MR !25996, authored and merged by John Thacker, fixes Windows software-update handling because WinSparkle can call `SoftwareUpdate::softwareUpdateEngaged()` from another thread while the method starts/stops a `QTimer`. Since a `QTimer` may only be started/stopped from its owning thread, the fix uses `QMetaObject::invokeMethod(..., Qt::QueuedConnection)` to enqueue the operation on the correct Qt thread.

**Implementation rule:** at boundaries from third-party/native callback threads into Qt UI/application objects, identify the owning thread before touching QObjects, timers, or UI state. Marshal work through a queued Qt invocation/signal when affinity is not guaranteed; do not rely on the callback's current thread.

**Confidence:** Very high. Merged correctness fix authored and merged by John Thacker with an explicit Qt thread-affinity rationale.

## Pair synchronization-object initialization with every clear/destruction lifecycle

Synchronization primitives can have platform-specific resources even when their public C representation looks trivially reusable. If an object is explicitly cleared/destroyed at the end of a session, it must be initialized again before the next session that uses it; conversely, omitting the clear may leak resources on platforms where the primitive owns native state.

Merged MR !26167, authored, approved, and merged by John Thacker, fixes capture-session toolbar locking by initializing its `GMutex` for each capture. The MR explicitly notes that a `GMutex` must be initialized once for each time it is cleared on some platforms, while it also must be cleared on some platforms to avoid a leak. A one-time initialization paired with repeated clears therefore had an invalid lifecycle.

**Implementation rule:** treat mutex/semaphore/condition initialization and clear/destruction as a matched lifecycle. If the enclosing session is reusable, reinitialize the synchronization primitive for every lifecycle after it has been cleared; do not assume zeroed/static storage remains a valid initialized object after explicit destruction.

**Confidence:** Very high. Merged cross-platform lifecycle fix authored and merged by John Thacker with the platform-dependent contract stated directly in the MR.

## Shared export/print options belong in frontend-independent argument state

When an output-format option is meaningful to more than one frontend, represent it in the shared print/export configuration rather than wiring it directly from one command-line parser into the formatter. That keeps the formatter contract frontend-neutral and lets Wireshark, tshark, and later callers expose the same behavior without parallel plumbing.

Merged MR !26114 adds compact JSON output. John Thacker specifically requested that the option be incorporated into `print_args_t` so the GUI could use it as well, rather than leaving it as a tshark-only flag. The accepted implementation carries `json_compact` through the shared print arguments and exposes the same setting in the JSON export dialog. Martin Mathieson also pointed the contributor to the existing `print_indent()` implementation, which had previously reduced PDML formatting overhead by avoiding repeated `fprintf()` indentation calls.

**Implementation rule:** put cross-frontend formatting/export policy in the common argument/configuration object consumed by the shared output code. Frontends should translate their UI or CLI controls into that shared state, not own separate formatter behavior. Before adding performance-specialized output code, check for an existing shared helper that already implements the same low-level operation efficiently.

**Confidence:** Very high. Merged master feature with explicit architectural review from John Thacker and implementation-performance guidance from Martin Mathieson.