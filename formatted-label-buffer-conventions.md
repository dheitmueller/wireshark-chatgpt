# Wireshark Formatted Label Buffer Conventions

This file records durable sizing rules for buffers that hold human-readable or resolved field values. Current upstream source remains authoritative.

## Size label buffers for the maximum formatted representation, not the source integer width

The storage required to render a numeric field is determined by all formatting modes that can be applied to it, including symbolic/name resolution, prefixes, separators, and annotations. A buffer sized only for the decimal/hex width of the underlying integer can truncate legitimate display output even though the numeric value itself fits comfortably.

Merged master MR !13337, authored and merged by John Thacker, fixes truncation of `BASE_OUI` labels. The existing intermediate numeric-label buffers were sized under assumptions appropriate to ordinary 32-bit and 64-bit numeric representations, but an OUI lookup can contribute a manufacturer name up to 64 bytes plus the numeric formatting. The accepted change introduces a shared `NUMBER_LABEL_LENGTH` of 80 bytes and uses it for the affected numeric-label paths.

**Implementation rule:** derive formatting-buffer capacity from the longest allowed formatted result, not from the bit width of the source field. When a display base can perform name resolution, include the maximum resolved-name length and all punctuation/numeric text in the bound. Prefer one named capacity shared by paths with the same formatting contract rather than several smaller ad hoc constants that can drift.

**Review rule:** when adding a new display base or resolution mode to an existing formatter, audit intermediate buffers as well as the final protocol-tree item buffer. Truncation can occur before the final destination even when that destination is large enough.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker with a concrete legitimate formatting mode exceeding the prior assumptions.