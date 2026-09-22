# Wireshark Qt Model Notification Conventions

This file records durable conventions for Qt model change notifications. Current upstream source remains authoritative.

## Preserve the owning parent when constructing indexes for `dataChanged()`

In a hierarchical Qt model, row numbers are meaningful only within their parent. A `dataChanged()` range built with root-level indexes for a child row does not identify the item that was actually edited, even if the numeric row and column values look correct.

Merged master MR !13264, authored and merged by Stig Bjørlykke, fixes the Advanced Preferences model by constructing both ends of the `dataChanged()` range with `dataindex.parent()`. The change also expands the range across all columns because editing a preference can alter presentation such as the font/status in columns other than the value column. Merged backports !13265, !13266, and !13267 carry the same fix to the supported release branches.

**Implementation rule:** model indexes emitted in change notifications must preserve the same parent/index hierarchy as the affected item. Do not reconstruct a child index with `index(row, column)` when that overload would place it under the root. If an edit can alter roles or presentation across multiple columns, notify the complete affected column range rather than only the editor column.

**Testing rule:** exercise edits on nested rows, not just top-level rows, and verify every dependent presentation role/column updates without requiring a model reset or unrelated refresh.

**Confidence:** Extremely high. Merged master correctness fix plus three stable backports, all carrying the same parent-aware `dataChanged()` construction.