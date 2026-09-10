# Wireshark Parser Recursion Conventions

This file records durable recursion and dissection-depth conventions extracted from upstream Wireshark merge-request review. Current upstream source remains authoritative.

## Treat unconstrained dynamic/open types as potentially recursive

When generated protocol structure allows a runtime-selected type whose possible targets cannot be bounded statically, absence of a known dependency edge is not proof that recursion is impossible. Conservatively route such paths through normal dissection-depth protection.

Merged release-branch MRs !24315 and !24316, authored and merged by John Thacker, change `asn2wrs` handling of unconstrained ASN.1 open types. Because an open type can carry values whose type is not known at specification time, generated dependency analysis cannot prove that it is outside a cycle. The accepted implementation therefore treats an otherwise-unresolved open type as cyclic with itself so generated dissection receives depth checks.

**Implementation rule:** if a dynamic dispatch/type mechanism can reach an unknown set of parser targets, assume recursive reachability unless the schema proves otherwise. Favor a bounded false positive in recursion protection over an unbounded parser path.

**Confidence:** Very high. The behavior is preserved in two merged stable branches by John Thacker and is justified from the ASN.1 open-type semantics in the MR rationale.

## Mutate dissection depth through checked helper APIs, including multi-level adjustments

Dissection depth is an invariant-bearing safety counter, not ordinary scratch arithmetic. Code that conceptually crosses several generated dependency levels should use a helper that applies the full adjustment with overflow/underflow checks and should undo the same amount through the matching helper.

Merged release-branch MRs !24311 and !24314, authored and merged by John Thacker, introduce/use `increment_dissection_depth_by_n()` and `decrement_dissection_depth_by_n()` rather than directly modifying `pinfo->dissection_depth` around calls. Besides centralizing overflow/underflow checking, the change corrects generated paths that had previously incremented and decremented by one level too many.

**Implementation rule:** do not directly add to or subtract from `pinfo->dissection_depth`. Express the semantic number of recursion levels through the checked depth helpers, and keep increments/decrements exactly paired across every return path.

**Confidence:** Very high. Merged safety API and generated-code corrections authored and merged by John Thacker on two maintained branches.
