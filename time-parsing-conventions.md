# Wireshark Time Parsing Conventions

This file records durable conventions for parsing absolute times, Unix-epoch values, and time-zone syntax. Current upstream APIs and supported-platform behavior remain authoritative.

## Parse numeric epoch representations in their native numeric domain

If an input format already defines a value as seconds from the Unix epoch plus an optional fractional component, parse that representation directly as an integer/fraction pair. Do not route it through calendar-time parsing (`struct tm`, `strptime()`, `mktime()`) unless the format actually contains calendar fields.

Merged master MR !11917, authored by Guy Harris, rewrites `unix_epoch_to_nstime()` accordingly. The accepted implementation parses the seconds with `ws_strtoi64()`, range-checks the conversion to `time_t`, and parses the fractional digits directly into nanoseconds. It removes the previous `strptime("%s")` path for two reasons called out in the MR: interpreting an epoch count via `struct tm` risks accidentally importing local-time semantics, and `%s` is not a Single UNIX Specification `strptime()` conversion. Release backports !11918 and !11920 preserve the same behavior.

**Implementation rule:** keep the parser aligned with the representation's semantic domain. Numeric epoch seconds are numeric epoch seconds, not a broken-down local calendar time. Parse the wide intermediate first, then verify that narrowing to the platform storage type is lossless.

**Portability rule:** do not depend on non-standard `strptime()` conversions merely because they exist on one libc. When Wireshark can parse the representation directly with its portable integer helpers, prefer the portable representation-level parser.

**Testing rule:** cover the target `time_t` width boundary, fractional-second forms, malformed input, and any intentionally unsupported domain (for !11917, negative epoch values were explicitly rejected at that point). A successful parse on a 64-bit development host is not sufficient evidence that the conversion is valid on every supported data model.

**Confidence:** Extremely high. The master change is merged and authored by Guy Harris, with two accepted stable-branch backports.

## Accept time-zone syntax only when its meaning is unambiguous and portable

A textual time-zone abbreviation is not necessarily a portable identifier. Abbreviations such as `PST`, `CEST`, or `WET` can be ambiguous, and supported platforms do not provide one portable API that converts an arbitrary abbreviation into the intended zone for a broken-down input time.

Merged master MR !11947 adds deterministic support for a `UTC` suffix in display-filter absolute-time literals. In follow-up discussion, Guy Harris explained why that result should not be generalized mechanically to arbitrary abbreviations: UTC has a single directly expressible meaning, while arbitrary abbreviations can map to multiple zones and have no general cross-platform conversion facility across the supported UNIX/Windows environments. The test deliberately keeps an unsupported abbreviation as a failure case. Merged !11951 subsequently extends the accepted time-zone syntax without relying on ambiguous abbreviation guessing.

**Parser rule:** support forms whose semantics can be defined consistently on every supported platform (for example UTC and explicitly represented offsets). Reject ambiguous or platform-dependent zone names rather than guessing from the host's locale/time-zone database.

**Review rule:** when adding a convenience spelling for a time zone, ask both whether the spelling uniquely identifies an offset/rule set and whether the conversion can be implemented with the same semantics on supported platforms. A libc accepting a spelling on one host does not by itself make that spelling part of Wireshark's portable filter language.

**Testing rule:** include at least one deliberately unsupported/ambiguous spelling in negative tests so later refactors do not accidentally turn host-specific libc behavior into accepted display-filter syntax.

**Confidence:** Very high. !11947 and !11951 are merged master parser changes; the portability/ambiguity rationale comes from substantive Guy Harris discussion on !11947.
