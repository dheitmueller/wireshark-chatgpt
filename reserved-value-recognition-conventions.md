# Wireshark Reserved-Value Recognition Conventions

## Encode the complete protocol pattern once

Merged master MR !9850 fixes TLS GREASE recognition. The previous bit test could incorrectly treat a near-miss value such as 0x1a2a as GREASE. The accepted change introduces one named TLS predicate containing the full patterned-value condition, and likewise centralizes QUIC's reserved transport-parameter formula.

**Rule:** for patterned reserved/sentinel values, encode the complete normative condition in one named helper and reuse it. Avoid repeated approximate bit tests whose accepted set is broader than the specification.

**Testing:** include near-miss values that satisfy only part of the reserved pattern, not just positive examples.

**Confidence:** High. Merged master correctness change with an explicit false-positive example and standards references.
