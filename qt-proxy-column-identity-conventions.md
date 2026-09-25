# Wireshark Qt Proxy-Column Identity Conventions

This file records durable model/view rules for preserving semantic column identity across Qt source models, proxy models, and views. Current upstream Qt code remains authoritative.

## Store semantic column identity in source-model coordinates

A displayed column number is presentation state. Filtering, hidden columns, proxy models, and view reordering can change which source-model field appears at that position. Long-lived behavior such as “filter by this column” or “draw a timeline for Start/Duration” should therefore be keyed to the semantic source column rather than whichever visual slot happened to be selected.

Merged master MR !9819, authored by John Thacker, fixes the Conversations dialog after timeline delegates stayed attached to fixed displayed column numbers while filtering/hiding moved different content into those slots. The accepted design applies the delegate broadly and decides whether to draw from the model's semantic Start/Duration columns. Roland Knall's review also sharpened the API rule that a column operation should remain column-oriented rather than requiring an arbitrary row-backed `QModelIndex`.

Merged master MR !9840, also authored by John Thacker, fixes the complementary filtering problem. A user's selected proxy column is mapped to the source-model column when the filter is established, so later column hiding does not silently retarget the filter. The mapping also works when no rows are visible and avoids repeated mapping during every row evaluation.

**Implementation rule:** store semantic/filter state in stable source-model coordinates and map from the proxy/view at the boundary where the user selects it. Use view/header facilities for presentation changes rather than letting those changes alter stored semantic identity.

**Qt API rule:** if an operation is conceptually about a column, keep its public contract column-oriented. Do not require an arbitrary row index merely to translate columns; a valid fully filtered model can have no row available.

**Testing rule:** hide or filter columns, reduce the proxy to zero rows, then change the filter or trigger delegates. Verify that the same source column remains selected and rendering does not migrate to unrelated data.

**Confidence:** Very high. Two merged master correctness fixes authored by John Thacker with substantive Qt model/view review by Roland Knall.
