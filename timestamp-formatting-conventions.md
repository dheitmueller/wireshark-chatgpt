# Wireshark Timestamp Formatting Conventions

This file records durable conventions for timestamp presentation APIs and precision selection. Current upstream source remains authoritative.

## Treat supported timestamp precision as a numeric domain, not a list of favored cases

If Wireshark defines timestamp precision as a contiguous range, formatting and command-line handling should model the complete range rather than special-case the historically common second, millisecond, microsecond, and nanosecond values. Hard-coded lists tend to leave centiseconds, deciseconds, or other valid precisions inconsistently supported across frontends and helpers.

A sequence of merged master MRs authored and approved by Guy Harris establishes this direction. !12030 and !12031 update signed/epoch time formatting to operate on `nstime_t` and support every precision from seconds through nanoseconds. !12033 adds common ISO 8601 conversion helpers, !12041 introduces a shared fractional-nanosecond formatter, and !12054 moves existing timestamp-to-string paths onto that common routine. !12060 then replaces remaining selected-resolution conditionals with precision-indexed tables or calculations and uses `WS_TSPREC_MAX` instead of a literal `9`. Finally, !12061 changes the `-t` option from enumerating a few allowed digits to parsing a numeric precision and validating it against the full supported range.

**Implementation rule:** when an enum or constant set defines a contiguous supported precision domain, derive bounds and table sizes from the shared domain (`WS_TSPREC_MAX`, `NUM_WS_TSPREC_VALS`, or their current equivalents) rather than spelling individual accepted values or magic maximums. Let user-facing parsers validate a numeric value against that domain instead of maintaining a second whitelist.

**Architecture rule:** centralize fractional-second and full-timestamp rendering in shared helpers. Callers should select semantic format and precision; they should not duplicate digit scaling, decimal-point handling, padding, or precision-specific branches.

**Testing rule:** exercise intermediate precisions as well as the endpoints and common 3/6/9-digit cases. A test matrix that only covers milliseconds, microseconds, and nanoseconds can miss exactly the inconsistencies this design removes.

**Confidence:** Extremely high. This is a coherent series of merged master changes authored and approved by Guy Harris, spanning shared APIs, call-site migration, UI/CLI precision handling, and removal of hard-coded resolution cases.


## Treat calendar conversion as fallible and suppress fractional output when the base time is unavailable

Platform calendar-conversion APIs can reject timestamps that the capture data type itself can represent. Formatting code must check the conversion result and must not print fractional nanoseconds as though a complete timestamp had been produced when the seconds component could not be represented.

Guy Harris's merged master sequence !1837, !1838, and !1843 fixes timestamp printing for negative/out-of-range values. The initial change stopped assuming `gmtime_s()` / `gmtime_r()` success; the follow-up avoided Windows `gmtime_s()` because its invalid-parameter behavior can invoke an exception handler; and !1843 prevents the nanosecond suffix from being emitted if conversion of the seconds failed. Stable backports !1839-!1845 carry the same fixes.

**Portability rule:** check the return contract of platform time-conversion routines even when the input type is wider than the platform calendar implementation.

**Presentation rule:** a fractional-second suffix is subordinate to a successfully formatted base timestamp. If the seconds cannot be represented, report/fallback coherently rather than displaying an orphaned fractional component.

**Platform rule:** avoid CRT APIs whose invalid-input contract can terminate or invoke process-level handlers when a non-terminating alternative is sufficient for Wireshark's use.

**Confidence:** Extremely high. Coherent merged master and stable-branch series authored by Guy Harris.
