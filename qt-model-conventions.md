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

## Preserve source-column identity across filtered and empty proxy views

Merged master MR !9840 (John Thacker) resolves the selected visible proxy column into a source column at filter-selection time; it must not depend on having a valid row index after the current filter removes all rows. Roland Knall asked that the exceptional empty-view path not impose a repeated cost in the normal case. Mapping only when the filter changes also avoids `mapToSourceColumn()` work for each `filterAcceptsRow()` call. Related accepted MR !9819 maps semantic Start/Duration columns through the proxy for timeline painting, uses the base delegate for unrelated columns, and preserves a column-oriented API after Roland's review.

**Implementation rule:** persist semantic *source* column identity; translate between source and proxy at the UI boundary, not inside each hot-path row operation. Do not depend on a live row for an operation whose meaning is only about columns. Keep column-centric public APIs column-centric.

**Tests:** hidden/filtered columns, zero visible rows while changing filters, and timeline delegate behavior after column changes.

**Confidence:** Very high: merged master work and implemented review from John Thacker and Roland Knall.

## Cache validity needs the version of its rendering rules

John Thacker's merged !9737 changes Qt `colorized()` to require both `colorized_` and `color_ver_ == rows_color_ver_`; old cached colors should not be considered current after rule changes. Merged !9741 removes an unnecessary packet-list `resize()` that could hide the scrollbar/minimap after column updates, while !9740 and backports correct the minimap final-row fencepost.

**Rule:** Track visual-cache generation, not simply whether output was once computed. Let the Qt view/layout control scrollbar geometry instead of resizing the packet list to column totals. Test off-screen recoloring, add/remove columns, and the final row. **Confidence:** High: merged John Thacker corrections.

## A selector's enum values must match its combo-box index mapping

Merged master !9663 by John Thacker and release-4.0 backport !9664 fix the Show Packet Bytes dialog: `ShowAsRustArray` had been declared out of order relative to the presentation combobox. When the UI treats an integer enum as a combo-box index, the two sequences are an implicit ABI.

**UI rule:** avoid relying on index equality when an explicit `QVariant` item value is feasible. If index mapping is retained, document and test that enum order, displayed item order and conversion code agree when options are inserted or alphabetized. **Confidence:** high; merged John Thacker fix and backport.

Merged master !9693 and backports !9695/!9696 make the RTP Stream Dialog packet-loss sort compare `rtpstream_info_calculate()` results rather than a stale or different cached member. **Sorting rule:** the comparator must use the displayed semantic metric and consistent calculation on both rows. **Confidence:** high; merged master and backports.

## A custom-column field identifier is not the complete column identity

In merged master !9476 and release-4.0/3.6 backports !9491/!9492, Stig Bjørlykke corrects `column_prefs_has_custom()`: an existing column with the same expression but a *nonzero occurrence* is not equivalent to the default-occurrence custom column proposed by “Apply as Column”. The updated duplicate check requires `cfmt->custom_occurrence == 0` before suppressing that UI action.

**Model/UI rule:** include modifiers such as occurrence index in equality semantics when they change what a column actually displays. A string-equal filter expression alone is insufficient to decide whether the user already has the requested column. Regression-test adding a default column alongside an existing field with a nondefault occurrence. **Confidence:** high, merged master fix and two accepted backports.

## Sort traffic data by the typed model role, not by speculative numeric conversion

John Thacker's merged master !9438 (release backport !9450) corrects sorting of GeoIP country/city and numeric Endpoint/Conversation values. `QVariant::canConvert<double>()` tests whether the **type** has a conversion path, not whether this **particular value** is a number; even a nonnumeric string can pass. `TrafficDataFilterProxy` instead selects `ATapDataModel::UNFORMATTED_DISPLAYDATA` as its sort role and lets `QSortFilterProxyModel` compare appropriately typed values.

**Rule:** expose a stable, typed semantic sort role; never promote arbitrary display text to a number solely because `canConvert` succeeds. Test mixed numeric, percentage and textual fields. **Confidence:** very high (merged John Thacker master fix plus backport).

## Preserve displayed semantics when filtering typed traffic-table values

Merged master !9447 replaces blanket integer conversion for Endpoint/Conversation filter comparisons with Qt-version-aware type handling. On Qt 6 it uses `QVariant::compare` with data on the left (conversion direction matters), explicitly handles time-only inputs for displayed `QDateTime` starts, and compares percentages at the two-decimal precision actually shown. Qt 5 requires a separate type-dispatched fallback. A filter expecting the visible time should not require users to type a hidden date, although time-only semantics are ambiguous for captures spanning midnight.

**Rule:** filter comparison must respect the model's semantic type and the presentation contract. Validate conversion failures, doubles, strings, dates/times, negative numbers, and rounded equality on both Qt 5 and 6. Do not silently coerce all filter input to `long long`. **Confidence:** high, merged John Thacker implementation; note its explicit time-only limitations.

Merged master !9437 (backport !9449) also fixes filter-menu column mapping when the Conversation ID and total columns are hidden: map **displayed** positions to canonical source positions by accounting for **absent** columns, not present ones. Merged !9419 (backport !9426) fixes `UatModel::appendEntry()` rejecting row data based on current **row count** rather than **column count**. **Rule:** maintain dimensional invariants explicitly; rows, columns, proxy positions and canonical field IDs are not interchangeable. **Confidence:** very high.

## Bound the memory/time tradeoff when sorting requires redissection

Merged master !9222, authored and merged by John Thacker, makes expensive packet-list column-text sorting depend on a bounded, configurable cached-row budget. Columns whose values need packet dissection (including custom columns and even some built-in address columns) otherwise repeatedly dissect packets during an `O(N log N)` comparison sort. Caching the values reduces dissection work toward one pass over the packets; if the visible row count exceeds the configured budget, the GUI disables sorting **for those expensive columns** rather than silently doing unbounded work or memory allocation. Frame-metadata-only columns such as packet length do not need this restriction. The accepted code also caps the unsigned preference to Qt model's signed row index domain.

**UI-performance rule:** model whether a derived column requires expensive extraction, cache at the semantic invalidation boundary, and set an explicit memory budget. Preserve cheap sortable columns and communicate when an expensive sort is unavailable; don't apply one arbitrary limit to every column. Exercise large captures, custom/address columns, filter transitions, cache invalidation and signed/unsigned row-count limits.

**Confidence:** Very high: John Thacker's merged fix and quantified regression analysis in discussion.

## Separate colorization from packet-column extraction in a bounded cache

Merged !9143 (Tomasz Moń) replaced unbounded per-record QString column retention with a bounded QCache keyed by frame number. Coloring a row no longer populates expensive column strings merely as a side effect, and capture/model reset invalidates cached column values. Gerald Combs asked about UTF-8 versus QString/UTF-16 conversion boundaries; keeping the type returned by PacketListRecord::columnString avoids conversions during normal visible-column requests.

**Performance caveat:** a cache of 500 rows makes memory predictable but can force repeated dissections during O(N log N) sorting. John Thacker argued sorting by expensive columns remains useful, suggested a configurable budget and a status-bar explanation rather than a disruptive dialog, and implemented the later merged !9222 policy (already described above). **Rule:** reason separately about normal scrolling, colorization, sorting and invalidation; constrain costly operations using a user-visible budget rather than making all sorting silently slow. **Confidence:** very high for merged code plus follow-up reviewer guidance.

## A blank-area context-menu event may supply a negative model row

Merged !9105 (Chuck Craft) fixes a Conversations-table crash when the user right-clicks below the last row. The selected row is `-1`, and a guard that checked only `row >= storage_->len` permitted invalid indexing. The accepted `ui/qt/models/atap_data_model.cpp` guard tests `row < 0` as well as the upper bound and missing storage.

**Rule:** validate both sides of an index range when converting UI coordinates or selection indexes to direct container offsets. Test context menus on cells, empty viewport space and completely empty models; the absence of an item is a normal UI event, not malformed model state. **Confidence:** high, merged one-line crash fix.

## A list-reordering feature must preserve view selection and model notification

Merged Qt **!9011** adds Move Up/Down controls to IO Graphs. Its handlers swap entries in the `ioGraphs_` collection, rebuild the UAT view as needed and keep controls disabled without a valid row. **Gerald Combs** supported the feature while flagging accidental UI text and warning that adding controls further crowded the dialog.

**Rule:** when adding model-backed row reordering, enforce end-row boundaries, preserve the logical selected item through the move, and keep model/view state and button enablement synchronized. Review toolbar density and existing accessibility/interaction conventions as well as correct data movement. A small UI addition can deserve a broader layout evaluation without blocking useful behavior.

**Confidence:** High for merged implementation; maintainers did not establish an absolute preferred layout.

## Root and synthetic protocol-tree nodes may not have field metadata

Merged **!8915** (John Thacker) fixes an Expert Info dialog crash: the synthetic root of `ProtoTreeModel` has no associated `field_info`, so `PNODE_FINFO(node->protoNode())->hfinfo->id` was an invalid dereference. The accepted check tests `PNODE_FINFO(...)` before accessing its `hfinfo`; **Jim Young** confirmed the original crash could no longer be reproduced.

**Rule:** Qt protocol-tree search, filtering and navigation must handle root/placeholder nodes separately from real packet fields; do not assume every visible or traversable model node owns an `hfinfo`. Include expanded-root and non-field nodes in UI regression tests. **Confidence:** very high, merged crash repair with independent reporter reproduction.
