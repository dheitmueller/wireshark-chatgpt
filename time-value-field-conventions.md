# Wireshark Time-Value Field Conventions

This file records durable conventions for representing time and duration values in protocol state and display-filter fields. Current upstream source remains authoritative.

## Prefer semantic time types over floating-point seconds

Protocol times and durations that participate in display filtering, comparisons, or long-lived state should use Wireshark's semantic time representation (`nstime_t` and the corresponding time field type such as `FT_RELATIVE_TIME`) when practical rather than being modeled as `float` or `double` seconds.

Merged MR !15887, authored by John Thacker, improves MATE timestamp precision by changing `float` storage to `double`. During review, Guy Harris gave the more durable direction: floating-point `==` and `!=` tests can be unreliable because many decimal numerical values cannot be represented exactly in binary, while a future `FT_RELATIVE_TIME` representation avoids that issue by storing integer seconds and integer nanoseconds. The merged float-to-double change is therefore useful as an immediate precision fix, but not the ideal semantic representation for filterable time values.

**Implementation rule:** use Wireshark time-domain types when a value is semantically a timestamp or duration. Do not choose floating point merely because the value is commonly printed as fractional seconds. If an interim float/double representation must change precision, consider display-filter compatibility and equality behavior explicitly.

**Review rule:** treat changes in time representation as user-visible semantic changes, not only storage-width changes. Audit display filters, formatting, serialization, and comparison behavior when precision or type changes.

**Confidence:** Very high. Merged master change by John Thacker with explicit representation and filter-semantics guidance from Guy Harris.
