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


## Zero-width elements are a legitimate exception to strict progress

Merged release-branch MRs !6116 and !6117 show that ASN.1 PER NULL values can validly consume zero bits. Their fix bounds repeated zero-width work and narrows the known ATN-ULCS cardinality instead of requiring every semantic item to advance.

**Implementation rule:** require strict cursor movement only where the encoding promises it. For valid zero-width elements, bound work by count, cardinality, or another structural budget.

The earlier ZigBee ZCL change corrected by Guy Harris in !6162 was merged master !6135, with stable counterparts !6136 and !6137. Those changes used the progress comparison backwards and therefore rejected normal forward movement. Treat !6135-!6137 as negative regression evidence and !6162 as the authoritative correction.

**Testing rule:** every no-progress guard needs both a malformed/stationary case and an ordinary positive-progress case.

## A malformed variable-length integer must not return a stationary cursor

A decoder failure whose encoded-length result is zero is not equivalent to a successfully decoded zero-width semantic item. If an enclosing parser treats a helper's returned offset as its next cursor, returning the same offset on malformed input can create an infinite loop.

Merged master MR !5626 hardens Kafka so `tvb_get_varint()` failure reports expert information and returns the captured-length cursor instead of the unchanged input offset. Merged release-3.6 !5629 preserves the same behavior. Guy Harris's merged release-3.4 MR !5657 is especially strong corroboration: it applies the same termination rule across Kafka's varint-backed helpers while still returning `offset + len` when a valid encoding consumed bytes but its decoded value was semantically invalid.

**Implementation rule:** distinguish a legal zero-width grammar construct from a decoder failure that reports “consumed zero bytes.” When failure would otherwise leave an enclosing cursor stationary, terminate or propagate an explicit failure that the caller must handle; do not fabricate a maximum encoded length merely to force progress.

**Review rule:** audit every caller of a helper whose failure sentinel can also be interpreted as a length or cursor delta. The helper and the enclosing loop must agree on whether the sentinel means “no bytes consumed,” “stop,” or a valid zero-width item.

**Confidence:** Extremely high. Merged master behavior plus maintained-branch backports, including a Guy Harris-authored backport.

