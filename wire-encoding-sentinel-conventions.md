# Wireshark Wire-Encoding Sentinel Conventions

This file records durable conventions for preserving wire-level special values across representation changes. Current upstream source remains authoritative; these notes are a curated review cache.

## Recognize wire-format sentinels before lossy conversion

When a protocol assigns semantic meaning to a particular raw encoding, test that encoding before converting the value into a coarser representation. Rounding, truncation, unit conversion, or narrowing can collapse a legitimate non-sentinel value onto the sentinel value and silently change its meaning.

Merged MR !12708, authored and merged by John Thacker, fixes the NTP time decoder. In the NTP fixed-point representation, an all-zero timestamp means that the timestamp is unavailable. The previous code converted the fractional field to nanoseconds before deciding whether the value was zero. A tiny but valid nonzero timestamp such as one fractional unit (`2^-32` seconds) truncates to zero nanoseconds, so it was incorrectly reported as unavailable. The accepted implementation tests the raw seconds and fractional words for the all-zero sentinel first and only then performs the scale conversion.

**Implementation rule:** preserve and classify protocol-defined sentinel/special encodings in their native representation before any lossy transform. Only after the special-value decision has been made should the ordinary value be rounded, scaled, narrowed, or converted into Wireshark's internal units.

**Review rule:** when changing timestamp, fixed-point, scaled-integer, or unit-conversion code, explicitly test values immediately adjacent to each special encoding as well as the special encoding itself. In particular, include the smallest nonzero representable value when zero has protocol semantics.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker, with a concrete edge case showing semantic collapse during conversion.
