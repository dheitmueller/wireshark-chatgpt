# Wireshark Qt View Semantics Conventions

This file records durable conventions for Qt view models, derived presentation caches, and user-facing copy/export behavior. Current upstream Qt code remains authoritative.

## Bound expensive derived-data caches to the working set

Derived packet-list presentation data can be expensive to compute and expensive to retain. Cache it according to the access pattern that actually needs it, and avoid populating the cache during unrelated traversals.

Merged master MR !9143 replaces per-record retained packet-list column strings with a bounded Qt cache. It also separates column dissection from colorization so a colorization pass does not populate and churn column text that it never consumes. Review discussion between Tomasz Moń, John Thacker, and Gerald Combs explicitly recognizes that whole-column sorting is a different workload: it may need values for every visible row and can become very slow if a small working-set cache repeatedly evicts them.

**Implementation rule:** isolate expensive derived computations by consumer. A pass that needs colorization should not implicitly populate a column-text cache, and a working-set cache should not be assumed to make a whole-dataset operation efficient.

**Resource rule:** when an operation such as sorting inherently needs derived data for the full visible set, make its resource behavior explicit through an appropriate limit or sizing policy rather than relying on accidental cache behavior.

**Confidence:** Very high. Merged master performance/correctness change with direct maintainer discussion of cache semantics and sorting behavior.

## Copy and export from the model that defines what the user sees

When a command means “copy/export this view,” it should use the view's filtered/proxy model rather than bypassing it for the underlying source model. Otherwise hidden or filtered-out rows can reappear in the exported data.

Merged master MR !9120 fixes TrafficDialog CSV, YAML, and JSON copying by iterating the widget's active model instead of its underlying data model. The previous behavior ignored filtering and copied rows the user could not see.

**Implementation rule:** choose the data model from the user-facing operation's semantics. View-oriented copy/export should honor filtering, ordering, and visibility represented by the proxy/view model. Access the raw source model only for an operation explicitly defined to export all underlying data.

**Confidence:** Very high. Merged master correctness fix with a direct mismatch between filtered display and copied output.

## Sorting semantics should follow the representation the user is actually viewing

Merged master MR !6859, authored by Guy Harris, changes the Conversations and Endpoints address-column comparators so name-resolved views sort by the resolved display text; when name resolution is off they retain raw address ordering. Guy's MR rationale is explicit that displaying resolved names while sorting by the underlying addresses gives unexpected results and makes names harder to locate.

Merged master MR !6855 independently changes packet-list sorting to sort only the current `visible_rows_` rather than the complete physical-row collection before reconstructing the filtered view. The submitter reported that filtered sorting of a roughly 512 MB capture dropped from multiple minutes to under a second.

**Implementation rule:** for a view-level sort, define ordering over the active presentation model and representation. Honor filtering and visibility, and when a display mode substitutes a resolved textual identity for the raw value, compare the semantic value the user actually sees unless the UI explicitly promises raw-key ordering.

**Performance rule:** do not pay whole-capture sorting cost when only the filtered visible set participates in the view.

**Confidence:** Extremely high for resolved-name ordering because !6859 was authored by Guy Harris; high for visible-row scoping from merged !6855 with concrete large-capture performance evidence.
