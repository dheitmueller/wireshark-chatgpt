# Wireshark Numeric Field Semantics Conventions

## Keep measurable quantities numeric

Release-3.6 MR !9882 contains direct Gilbert Ramirez review that a TECMP voltage should be an FT_DOUBLE rather than an FT_STRING so users can filter numerically. Merged master MR !9887 implements that direction with proto_tree_add_double() and voltage units.

**Rule:** when a protocol value is semantically numeric, store the numeric value in the field and use Wireshark unit/presentation metadata for display. Do not preformat measurements as strings merely to add units or decimal punctuation.

**Testing:** verify both displayed units and numeric display-filter comparisons.

**Confidence:** Very high. Direct maintainer review followed by a merged master implementation.
