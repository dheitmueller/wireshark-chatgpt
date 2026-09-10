# Wireshark Parser Control-Flow Conventions

This file records durable parser control-flow conventions extracted from accepted upstream Wireshark changes. Current upstream source and API documentation remain authoritative.

## Rewrite control flow around the current API contract instead of preserving obsolete sentinel behavior

When a parsing/search helper changes from returning a special sentinel value to returning success/failure separately from an output offset, callers should be rewritten around the new contract. Do not manufacture the old sentinel behavior merely to preserve an existing loop shape.

Merged MR !24650, authored and merged by John Thacker, fixes SIP Security Mechanism parsing after conversion to `tvb_find_uint8_length()`. The older code relied on a failed search returning `-1` as an offset and used that value as part of the loop-entry logic. The newer helper reports success separately and leaves the output offset at the line boundary when the delimiter is absent. The accepted fix changes the parsing loop to a `do ... while` structure, removes obsolete failure emulation, and uses the helper's actual postcondition.

**Implementation rule:** whenever a tvbuff/parser helper is replaced or its return convention changes, audit surrounding loop termination, first-iteration behavior, and failure paths—not just the function call. Express the algorithm using the new API's status and output semantics rather than preserving control flow that depended on an old sentinel.

**Confidence:** Very high. Merged master parser correction authored and merged by John Thacker with the obsolete sentinel dependency explicitly documented in the MR.
