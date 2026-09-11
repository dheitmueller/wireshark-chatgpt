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

## Drive GUI consumers from the authoritative state transition, not connection order or duplicate proxy signals

When several views depend on shared application state, connect them to the signal that means the authoritative model/manager has actually completed the relevant transition. Do not make correctness depend on the order in which Qt signal connections happened to be created, and do not send a second synthetic signal merely to force downstream refreshes.

Merged MRs !26260, !26262, !26265, !26270, and !26274 form a coherent cleanup authored and merged by John Thacker. `InterfaceFrame` had depended on its slot running after `InterfaceTreeModel` happened to process the same interface-list signal; !26260 instead connects display refreshes to the model's `modelReset`, where the row count is known to be current. !26262 removes redundant dialog signals that caused extra GUI updates, including updates occurring before the actual interface refresh completed. !26265 routes interface-list change notification from the `InterfaceListManager`, described as the single source of that information, through `MainApplication` so consumers can subscribe without depending on a not-yet-visible `MainWindow`. !26270 avoids preference-triggered refreshes until the initial preference value is known, preventing startup work and side effects before initialization is complete. !26274 removes another duplicate refresh path because the manager already listens to the canonical preferences-changed signal.

**Architecture rule:** identify one authoritative owner for shared state and one event that represents completion of each meaningful state transition. Views and secondary controllers should subscribe to that transition or to the model reset it causes, rather than reproducing the refresh decision or relying on QObject construction/connection order.

**Lifecycle rule:** distinguish initialization from later changes. Do not run normal "changed" reactions until the baseline state is known, especially when those reactions trigger expensive interface scans, statistics, UAC prompts, or other externally visible work.

**Confidence:** Very high. Five merged master changes by John Thacker converge on the same event/lifecycle architecture and document concrete failures caused by ordering and duplicate notifications.

## Prefer explicit Qt signal connections over name-based auto-connect slots

For application code maintained in C++, connect widget signals explicitly in the owning class rather than depending on Qt Designer's `on_<object>_<signal>()` naming convention. Explicit `connect()` calls make the relationship visible to readers and tooling and are less fragile when widgets, object names, or slot names are refactored.

Merged MR !23875, authored and merged by Roland Knall, replaces the auto-connect slot naming convention in `WelcomePagePreferencesFrame` with explicit constructor `connect()` calls. The MR cites Qt documentation recommending explicit connections and notes that they match patterns already used elsewhere in Wireshark.

**Implementation rule:** when adding or modernizing Qt preference/UI classes, prefer explicit typed signal/slot connections. Do not rely on a magic slot name as the only expression of a behaviorally important connection.

**Confidence:** High. Merged master GUI cleanup by an authoritative Wireshark Qt maintainer, aligned with upstream Qt guidance and existing project practice.


## Register transformed payload buffers used by selectable tree fields

In the local `djh-10bit` cleanup based on `0e2f87ad739b06c12cbd60ed25ff656c527e5ac1` (2026-09-09), removing `add_new_data_source()` from `st291_get_8bit_payload_tvb()` caused a user-observed regression: selecting a VANC protocol such as ST 12-2 no longer displayed its payload in Packet Bytes. The byte-oriented payload dissectors construct tree items against the converted child TVB. `DataSourceTab::selectedFieldChanged()` looks up `fi->ds_tvb`, and `findDataSourceViewForTvb()` matches that TVB against registered data-source views by pointer identity. Merely attaching real data as a child TVB does not create a Packet Bytes view for it.

Restoring registration of `ST 291 UDW Bytes (bits 7..0)` is the targeted correction. Keep the packed 10-bit source and converted byte source distinct: their offsets and representations differ. Do not remove the converted source as a redundant tab without implementing and validating a mapping for tree selection/highlighting.

**Regression check:** select both the VANC protocol root and its child fields and verify the appropriate payload view and byte highlighting. Separately verify that manual word interpretation survives tree selection and tab changes.

**Evidence/scope:** user-observed regression and local source tracing; this is a prototype-specific finding, not an upstream-reviewed design rule. GUI confirmation of the correction is pending.


## Prototype: show data sources for the selected payload

On 2026-09-09, Devin requested trying selection-scoped tabs because an ST 2110-40 frame containing four ANC packets produced many repeated/truncated tab labels. The local `djh-10bit` prototype now adds an optional owner-field hint to `data_source`, via `set_data_source_owner()` / `get_data_source_owner()`. Existing sources default to no owner and retain their usual visibility. This is an additive API extension; the ST 291 subdissector context and dispatch contract are unchanged.

ST 2110-40 associates the packed source and sources created during each payload's subdissection with that ANC packet's tree field. A TRY/FINALLY also assigns the hint if payload decoding throws, and captures additional native/Lua decoded sources such as OP-47 without protocol-specific GUI code. The owner is borrowed from the same dissection; it does not transfer buffer ownership.

The Qt view retains all registered sources/widgets and only changes tab visibility. Selecting a decoded source selects its owner group; original transport fields resolve by the smallest containing owner range on the same TVB. Ungrouped sources stay visible; deselection or selection outside any payload hides grouped tabs. Retaining widgets preserves manual word-interpretation state. The four-payload case should therefore expose only the selected payload's packed/byte views and any further derived views, alongside the original packet.

Validation: full local build and existing ST 2110-40 decode-as/timecode regression passed. A temporary offscreen Qt harness compiled the actual visibility methods and exercised four owners, transport roots/children, decoded fields, OP-47, unrelated selection, deselection, and retained widgets. Full interactive GUI validation remains pending. This design is experimental and not upstream-approved.


## Isolate personal configuration during GUI development tests

Never launch a development Wireshark or Stratoshark build against the user's normal personal configuration during automated or exploratory GUI testing. Use a dedicated temporary directory via `-P persconf:<temporary-path>`. This isolates `recent`, `recent_common`, preferences, geometry, and other mutable profile state.

On 2026-09-10, an automated development-build launch used the normal profile. Qt temporarily resized seven packet-list columns to its 21-pixel minimum while the final Info column stretched to 1576 pixels. `PacketList::sectionResized()` records any resize received while the packet list is visible, including programmatic layout changes; the source explicitly notes that it cannot distinguish stretched values from manual changes. A later profile-state save persisted those transient widths. The development build's accessibility crash occurred earlier than the file write, so the crash itself was not the writer.

This was not caused by the Packet Bytes/data-source patch: that patch series changes only dissector, data-source, DataSourceTab, and HexDataSourceView files, with no packet-list, header, layout, or recent-settings changes. The persistence behavior exists in upstream packet-list code and can affect other developers or automated runs that share a real profile, although normal interactive use should not ordinarily enter the transient minimum-width state.
