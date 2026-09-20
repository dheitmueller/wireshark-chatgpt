# Wireshark UI Model-State Conventions

This file records durable ownership and synchronization conventions for UI models layered over mutable shared state. Current upstream source remains authoritative.

## Do not maintain independent derived model state over one mutable backing store

A model that reads a shared backing object directly while also maintaining private row-indexed metadata such as dirty flags, validation errors, or cached indices cannot safely coexist with another independently mutable model over the same storage. One model can change the backing rows while the other's derived arrays still describe the old shape, producing stale indices or crashes.

Merged master MR !15555, authored and merged by John Thacker, fixes I/O Graph crashes caused by multiple `UatModel` instances accessing the same UAT. The accepted design creates one shared I/O Graph model owned by the main application, lets all dialog views observe that model's row/reset signals, and reloads the model when the UAT is replaced outside the model (for example by a profile change).

**Architecture rule:** when a UI model owns derived state that must remain index-aligned with a mutable shared data source, centralize mutation and derived-state ownership in one model or otherwise provide an explicit synchronization protocol. Multiple views may share a model; multiple unsynchronized models must not each assume exclusive control of the same raw storage.

## Invalidate or detach views when the resource context they represent ends

A long-lived shared model can outlive an individual dialog or capture-file context. Views representing an old context must not continue reacting to model changes that belong to a new context.

Merged master MR !15582, also authored and merged by John Thacker, handles I/O Graph dialogs that remain open after a capture file closes while the shared UAT model later changes for another file. The fix detaches/disables the stale dialog's model-dependent controls so subsequent shared-model updates do not mutate a view whose capture context has ended. Merged !15564 complements this lifecycle by applying pending I/O Graph UAT changes before profile switching, rather than relying only on modal preference-dialog behavior.

**Implementation rule:** tie each view's subscriptions and editable state to the lifetime of the resource context it displays. On file close, profile transition, backing-store replacement, or similar lifecycle boundary, either refresh the model/view coherently or disconnect stale consumers before new-context state can arrive.

**Review rule:** exercise multi-window and lifecycle sequences, not only one-dialog steady state: two views editing one table, profile change while a non-modal dialog is open, close/reopen a capture while a dialog persists, and model reset while pending changes exist.

**Confidence:** Extremely high. The primary fixes are merged master changes authored and merged by John Thacker and address concrete crashes/data-inconsistency paths across multiple dialogs and lifecycle transitions.

## Choose the model notification whose semantics match the scope of the change, and measure its scaling

A nominally more precise Qt model notification is not automatically cheaper. If essentially the entire visible model has changed, an API that enumerates or computes geometry for a huge index range can scale with the total model size even though the view can only display a small fraction of those rows. Prefer a notification whose semantics match the actual scope while preserving required view state such as selection and current row.

Merged master MR !15174, authored and merged by John Thacker, works around a Qt 6 `QAbstractItemView::dataChanged` pessimization in the packet list. For an all-packets refresh, Qt 6 computes the union of the changed-index viewport rectangles, causing measured cost to grow from about one second at 1.4 million packets to about nine seconds at 12 million. The accepted implementation emits `layoutAboutToBeChanged()` / `layoutChanged()` instead, which refreshes the view while preserving current/selected rows and remained around 5–8 ms in the reported tests. The same fix was then merged to release-4.2 as !15177 and release-4.0 as !15178.

**Implementation rule:** when a model-wide update is intentional, use a model notification that expresses model-wide invalidation without forcing per-index work merely for precision the consumer cannot exploit. Verify which user state each notification preserves before substituting reset/layout/data-change signals.

**Performance rule:** benchmark model notifications at realistic large capture sizes and reason about the library implementation's complexity, not just Wireshark's own loop count. A call that looks constant-sized in application code can trigger work proportional to millions of rows inside Qt.

**Confidence:** Very high. Merged master performance fix authored and merged by John Thacker, backed by concrete scaling measurements and subsequently carried to two maintained release branches.
