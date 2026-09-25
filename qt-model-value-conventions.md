# Wireshark Qt Model-Value Conventions

This file records durable conventions for Qt model data used by sorting, filtering, and display. Current upstream source remains authoritative.

## Keep presentation values separate from typed comparison values

A Qt model role used for sorting or filtering should expose a value whose type matches the underlying semantic domain rather than forcing consumers to reverse-engineer formatted display text.

Merged master MR !9438, authored and merged by John Thacker, changes the Conversations/Endpoints sort path to use the model's `UNFORMATTED_DISPLAYDATA` role and lets `QSortFilterProxyModel` compare typed `QVariant` values. It removes a manual `canConvert` branch that incorrectly treated arbitrary strings as numerically convertible.

Merged master MR !9447, also authored and merged by John Thacker, extends the same model to filtering across integers, floating-point values, strings, and date/time values. It preserves the visible rounding contract for percentages by comparing against the rounded numeric value corresponding to the display.

**Implementation rule:** give model consumers a stable typed role for semantic comparisons and a separate display representation when the UI needs formatting. Sorting and filtering should operate on the typed role unless the user-visible contract explicitly requires comparison against the displayed or rounded value.

## Type convertibility is not per-value conversion success

Qt's `QVariant::canConvert` reports whether the source type has a conversion path to a target type; it does not prove that the current value will convert successfully. A QString type can be convertible to double even when a particular string is not numeric.

**Implementation rule:** when conversion success depends on the current value, use an API that reports conversion success for that value or avoid conversion by preserving the value's semantic type in the model.

## Hidden/displayed column indices are not stable semantic identities

Merged master MR !9437, with release-4.0 backport !9449, fixes Conversation-table filtering after the optional Conversation ID column changed the displayed-column layout.

**Review rule:** when a view can hide, insert, or reorder columns, do not assume a displayed index is the same as the source-model semantic column. Centralize the mapping and test both states of every optional column.

**Confidence:** Very high. These are merged correctness fixes, with the main value-role changes authored and merged by John Thacker.
