# Qt Model/View Conventions

Durable conventions inferred from accepted Wireshark merge requests involving Qt model/view code.

## Flat models must honor the parent-index contract

A model that represents a flat table must return zero rows for any valid parent `QModelIndex`; only the invalid/root index owns the table rows. Do not assume this contract is irrelevant merely because Wireshark code does not directly call `rowCount()` with a valid parent: Qt view internals may do so while expanding or recursively traversing a `QTreeView`.

Merged MR !10287 fixed `ATapDataModel::rowCount()` accordingly after a crash could be triggered through `QTreeView::expandRecursively()` (for example by pressing `*`). The accepted implementation returns the backing row count only when `!parent.isValid()` and documents the flat-table semantics in the model header. This is a useful reminder that Qt virtual methods are framework APIs: implementations must satisfy the framework contract, not merely the call patterns visible in Wireshark source.

**Confidence:** High. The fix was merged to master, authored by John Thacker, approved and merged by Gerald Combs, and directly tied to a reproducible Qt view behavior.
