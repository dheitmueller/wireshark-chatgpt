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
