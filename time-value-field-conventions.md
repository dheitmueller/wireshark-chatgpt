# Wireshark Time-Value Field Conventions

This file records durable conventions for representing time and duration values in protocol state and display-filter fields. Current upstream source remains authoritative.

## Prefer semantic time types over floating-point seconds

Protocol times and durations that participate in display filtering, comparisons, or long-lived state should use Wireshark's semantic time representation (`nstime_t` and the corresponding time field type such as `FT_RELATIVE_TIME`) when practical rather than being modeled as `float` or `double` seconds.

Merged MR !15887, authored by John Thacker, improves MATE timestamp precision by changing `float` storage to `double`. During review, Guy Harris gave the more durable direction: floating-point `==` and `!=` tests can be unreliable because many decimal numerical values cannot be represented exactly in binary, while a future `FT_RELATIVE_TIME` representation avoids that issue by storing integer seconds and integer nanoseconds. The merged float-to-double change is therefore useful as an immediate precision fix, but not the ideal semantic representation for filterable time values.

**Implementation rule:** use Wireshark time-domain types when a value is semantically a timestamp or duration. Do not choose floating point merely because the value is commonly printed as fractional seconds. If an interim float/double representation must change precision, consider display-filter compatibility and equality behavior explicitly.

**Review rule:** treat changes in time representation as user-visible semantic changes, not only storage-width changes. Audit display filters, formatting, serialization, and comparison behavior when precision or type changes.

**Confidence:** Very high. Merged master change by John Thacker with explicit representation and filter-semantics guidance from Guy Harris.

## Name time encodings for their semantic source, not merely a shared epoch

Two protocols or file formats can happen to count seconds from the same epoch while still having different semantic contracts, accepted field sizes, diagnostics, or future evolution. An encoding identifier should therefore describe the format/protocol whose representation it implements when that is the real scope of the implementation, rather than implying that every use of the same epoch is interchangeable.

Merged master MR !15550, authored and merged by Guy Harris, renames `ENC_TIME_CLASSIC_MAC_OS_SECS` to an MP4-specific encoding while retaining the old name for backwards compatibility. Guy explicitly explains that MP4's epoch happens to match the classic Mac OS epoch, but another format using the same epoch may deserve a distinct value so unsupported-size diagnostics and semantics remain format-appropriate. Merged follow-up !15551 tightens the name further to `ENC_TIME_MP4_FILE_SECS`, making the format/file scope explicit.

**API rule:** when an exposed encoding name has become misleading, prefer a semantically precise replacement and retain a compatibility alias where practical. Do not collapse distinct wire/file-format concepts solely because their numeric epoch or representation currently matches.

## Epoch conversion abstractions must encode direction as well as magnitude

Generic epoch conversion helpers need an explicit signed/directional model. A scheme that stores only an unsigned number of seconds to subtract works for epochs before Unix but does not naturally represent epochs after Unix.

Merged !15574, authored and merged by Guy Harris, introduced a Zigbee ZCL time encoding for an epoch beginning in 2000. Later review by John Thacker identified that the implementation's epoch-delta convention was modeled after pre-Unix epochs and questioned the subtraction/sign handling for the post-Unix Zigbee epoch. That later review is important evidence that the abstraction itself must make conversion direction unambiguous instead of relying on call-site intuition.

**Review rule:** whenever adding a new epoch, test at least one value at the epoch, one value on each relevant side of the Unix epoch when representable, and the supported field widths. Audit whether the helper stores a signed offset, an epoch timestamp, or an unsigned magnitude plus an implicit operation; post-Unix epochs are a particularly useful test of whether the abstraction is sound.

**Confidence:** High for the semantic-naming rule (two merged Guy Harris changes with explicit rationale). High as a review caution for the signed/directional epoch rule: it is based on a merged implementation plus later substantive John Thacker review that exposed the abstraction risk, not on a later corrected MR in this batch.
