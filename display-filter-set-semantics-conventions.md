# Wireshark Display-Filter Set and Multi-Value Semantics

This file records durable conventions for set membership, quantifiers, and negation in Wireshark display filters. Current upstream display-filter language semantics remain authoritative.

## Define quantifiers and absence explicitly; do not derive them from scalar Boolean identities

A display-filter field can have zero, one, or multiple occurrences. Set-membership operators therefore need semantics over a collection of field values, not merely scalar Boolean rewrites that happen to look equivalent for a single present value.

Merged master MR !11517, authored and merged by João Valverde, fixes `all X in S`. For a multi-valued field `X = {x1, x2, ...}`, the intended meaning is that every field occurrence is a member of the set: `(x1 in S) AND (x2 in S) AND ...`. The prior implementation instead combined set elements in a way that could accept a field when no single set element matched all occurrences.

Merged master MR !11519, also authored and merged by João Valverde, fixes `A not in S` so that it follows Wireshark's inequality semantics rather than being implemented as the generic Boolean complement of `A in S`. In Wireshark, field absence and multiple field occurrences make `!=`/`not in` observably different from simply applying `not` to equality/membership; the accepted compiler/runtime therefore models the negative-membership operation directly.

**Semantic rule:** specify set operators in terms of quantification over the field's value collection, including the zero-occurrence case. Do not assume that `all`, `not in`, or inequality can be obtained safely by applying ordinary scalar Boolean identities to `any`/`in`/equality.

**Implementation rule:** preserve distinct VM/compiler operations when the language distinguishes quantifiers or absence behavior. Optimizations and rewrites must prove semantic equivalence for absent fields and multi-valued fields, not just for a single concrete value.

**Testing rule:** every set/membership operator should have regression coverage for at least one absent field, one single-valued field, and one multi-valued field, including cases where different occurrences match different set elements. Negated forms should be tested independently rather than inferred from the positive-form tests.

**Confidence:** Very high. Two adjacent merged master semantic fixes authored and merged by João Valverde directly define the intended multi-value membership and negative-membership behavior.
