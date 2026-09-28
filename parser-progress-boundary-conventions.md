# Parser Progress Boundary Conventions

This file records durable parser-progress rules from accepted Wireshark changes. Current upstream source remains authoritative.

## A parser result that controls an enclosing loop must advance monotonically

Merged master MR !6163 and stable MRs !6174 and !6175 show that progress must be checked both when computing a next offset and when accepting a nested parser's result. GDSDB validates the minimum decoded length, checks whether `offset + length` wrapped, and rejects an opcode handler result that is not strictly greater than the saved input offset. BP changes error returns so the caller does not immediately revisit the same packet position.

Guy Harris's merged master MR !6162 provides the clearest predicate: after an element parser returns, the non-progress condition is `new_offset <= old_offset`. An earlier change had inverted that comparison and therefore rejected normal nonempty elements.

**Implementation rule:** when a helper return value drives an outer loop, strict forward movement is part of the helper contract. Validate arithmetic before producing the next cursor and reject a result that moves backward or remains unchanged.

**Testing rule:** test both a malformed/no-progress input and a normal positive-progress input. Cursor-safety changes are especially vulnerable to reversed comparison operators.

**Confidence:** Extremely high. Merged master fixes, including a direct Guy Harris correction, plus accepted stable backports.

## Bound work-producing input values to a useful domain

Merged MRs !6166 and !6167 add a finite traversal limit to RTMPT AMF length parsing. Merged MRs !6168 and !6169 clamp WAP variable-length values before those values participate in later size arithmetic.

**Implementation rule:** for decoded counts, lengths, or loop drivers, validate not only representability but also whether the value lies in a practical domain for downstream parsing. A representable maximum-width integer can still be an unsuitable size or iteration count.

**Confidence:** High. Accepted fixes applied to maintained release branches from corresponding master changes.
