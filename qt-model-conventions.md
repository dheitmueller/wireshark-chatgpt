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

## Traverse the proxy model when an operation is defined by the currently visible rows

A source model represents the underlying data set; a filter/sort proxy represents the user's current view of that set. Export, map-generation, or other actions whose semantics are “what is currently shown after filtering” must iterate the proxy rather than bypassing it and walking the source model directly. When domain-specific roles live on the source model, map each proxy index back to its source index for the data lookup.

Merged master MR !14274, authored by John Thacker and merged by Anders Broman, fixes GeoIP map generation from the Conversations/Endpoints traffic tabs. The accepted code iterates `TrafficDataFilterProxy::rowCount()`, converts each proxy index with `mapToSource()`, and then reads the underlying `ATapDataModel`. It also separates helpers that return the proxy from helpers that return the source model, making the semantic distinction explicit in call sites.

**Implementation rule:** decide whether an operation is view-relative or data-set-relative. Use the proxy for visible membership/order and the source model for canonical domain state or source-only roles. Do not silently bypass filtering by iterating the source merely because its API is more convenient.

**Testing rule:** for view-relative export/action paths, apply a filter (and sorting where relevant) before invoking the operation and verify that the result contains the same rows—and, where semantically meaningful, the same order—as the visible proxy view.

**Confidence:** Very high. Merged master model/view correctness fix authored by John Thacker and merged by Anders Broman.

## Keep proxy/source column mappings consistent; use the view for presentation-only reordering

A proxy model that maps columns by their relative source-model order rather than by calling `mapToSource()` for each column has an implicit structural contract: views may omit columns, but the columns they retain must remain in the same relative source order. Violating that contract can make an apparently unrelated action such as sorting one visible column operate on a different source column.

Merged master MR !13995, authored by John Thacker and merged by Anders Broman, fixes sorting in the Manage Interfaces dialog. `InterfaceSortFilterModel` maintained its own ordered proxy-to-source column mapping; the dialog had a different relative column order, so clicking “Hide/Show” could sort by the device name and clicking the device name could sort by another field. The accepted change realigns the source column enumeration with the relative order used by the views and documents that a view which genuinely needs a different visual order should use `QHeaderView::swapSections()`.

**Implementation rule:** make the proxy/source mapping contract explicit. If a proxy assumes source-relative column order, keep all model/view column identities in that order and let views omit columns as needed. For presentation-only rearrangement, use Qt's header/view facilities rather than silently redefining source-model column identity.

**Review rule:** when enabling sorting or filtering on a model shared by several views, verify that each visible header maps to the intended source column after omissions and any presentation reordering. Do not infer correctness merely because the unsorted display looks right.

**Testing rule:** click-sort every visible column in each view that shares the model, especially views that omit columns. Verify both ascending/descending order and that the values being compared are from the header the user selected.

**Confidence:** Very high. Merged master model/view correctness fix authored by John Thacker; the failure mechanism and recommended `QHeaderView::swapSections()` alternative are documented directly in the accepted change.

## Do not assume a proxy model has one concrete source-model subclass when multiple backends are supported

A Qt proxy can expose a common view while being backed by different source-model classes that do not inherit from one another. Code that blindly casts `sourceModel()` to the implementation used on one platform or initialization path can compile cleanly and then crash when another legitimate backend is installed.

Merged master MR !13817, authored and merged by John Thacker, fixes a Windows crash when adding a capture pipe before the interface list is loaded. `InterfaceSortFilterModel` can be backed by either `InterfaceTreeModel` or `InterfaceTreeCacheModel`; the old path effectively assumed one concrete source type and called a member through the wrong cast on systems supporting remote capture. The accepted code uses `qobject_cast` to determine the actual source model and invokes only methods valid for that subtype.

**Implementation rule:** when a proxy supports multiple source-model implementations, treat `QAbstractItemModel` as the stable boundary and branch explicitly on actual supported subtype only where subtype-specific behavior is unavoidable. Do not encode a platform-dependent concrete-model assumption in a cast.

**Design rule:** if the same operation is genuinely common to all supported source models, prefer moving that capability behind a shared interface/role rather than proliferating subtype checks. When the models have intentionally different APIs, keep the dispatch explicit and safe.

**Testing rule:** exercise the proxy with each supported source model, including initialization states such as an empty/not-yet-loaded interface list. Platform-specific backend selection is part of the test matrix because that is where invalid casts often remain hidden.

**Confidence:** Very high. Merged master crash fix authored and merged by John Thacker, with the multiple-unrelated-source-model cause stated explicitly.