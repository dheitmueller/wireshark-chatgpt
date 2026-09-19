# Qt Model/View Conventions

Durable conventions inferred from accepted Wireshark merge requests involving Qt model/view code.

## Flat models must honor the parent-index contract

A model that represents a flat table must return zero rows for any valid parent `QModelIndex`; only the invalid/root index owns the table rows. Do not assume this contract is irrelevant merely because Wireshark code does not directly call `rowCount()` with a valid parent: Qt view internals may do so while expanding or recursively traversing a `QTreeView`.

Merged MR !10287 fixed `ATapDataModel::rowCount()` accordingly after a crash could be triggered through `QTreeView::expandRecursively()` (for example by pressing `*`). The accepted implementation returns the backing row count only when `!parent.isValid()` and documents the flat-table semantics in the model header. This is a useful reminder that Qt virtual methods are framework APIs: implementations must satisfy the framework contract, not merely the call patterns visible in Wireshark source.

**Confidence:** High. The fix was merged to master, authored by John Thacker, approved and merged by Gerald Combs, and directly tied to a reproducible Qt view behavior.

## Reuse a domain model across views instead of duplicating widget-owned copies

When several Qt views present and manipulate the same application-domain objects, prefer one reusable model/proxy-model implementation over separate widget-owned populations of the same data. Putting roles, filtering, updates, and edit behavior in the shared model keeps the views semantically aligned and prevents fixes or new capabilities from having to be reproduced in multiple independently populated widgets.

Merged MR !26533 moves the Capture Options dialog's interface list from its own `QTreeWidget` implementation to the existing `InterfaceTreeCacheModel` / `InterfaceSortFilterModel` path, extending those common models where needed. As a result, the main window, Manage Interfaces dialog, and Capture Options dialog all use the same interface-model code. Gerald Combs authored the refactor and John Thacker approved and merged it; Gerald also noted that the commonization was preparatory work for adding bookmark management to Capture Options.

**Architecture rule:** for multiple views of the same domain collection, centralize the domain representation and reusable filtering/edit behavior in a model (and proxy model where appropriate); keep each view focused on presentation and view-specific interaction. Avoid parallel `QTreeWidget`/manual-population implementations when a shared model can represent the same state.

**Confidence:** Very high. Merged architectural refactor authored by Gerald Combs and approved/merged by John Thacker, with an explicit goal of sharing one implementation across three UI surfaces.