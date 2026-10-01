# Fixed Header Boundary Conventions

Merged master MR !2787 fixes PTP signalling TLV parsing by requiring the complete fixed Length-and-Type header to fit inside the protocol-declared message length before either field is read.

Rule: a repeated TLV or record parser should enter an iteration only when every fixed header byte needed by that iteration is inside the enclosing protocol unit. Checking only that the cursor is below the end is insufficient.

Scope rule: when the protocol provides an authoritative message length, use that boundary for the inner structure rather than the containing buffer's total length.

Confidence: high. Merged malformed-input correctness fix with the fixed-header requirement made explicit in the accepted loop guard.
