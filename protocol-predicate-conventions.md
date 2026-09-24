# Wireshark Protocol Value Predicate Conventions

This file records durable conventions for implementing specification-defined value classes and sentinel-pattern tests. Current protocol specifications and upstream Wireshark source remain authoritative.

## Validate the arithmetic domain before applying subtraction or modulo patterns

Protocol specifications often define reserved or greased value families with arithmetic formulas. Translating those formulas directly into unsigned C arithmetic can classify values outside the formula's domain if an initial subtraction wraps around.

Merged master MR !11428 fixes the QUIC GREASE predicate used while displaying transport parameters. The predicate tested `((x - 27) % 31) == 0`; for values below the formula's base, unsigned subtraction could wrap and create a false match. The accepted implementation first verifies that the value is above the base before subtracting. John Thacker explicitly endorsed the fix because wraparound was not desired. Merged release-4.0 !11429 and the release-3.6 GREASE cleanup !11433 preserve the same direction on maintained branches.

!11433 also replaces duplicated ad-hoc GREASE expressions with named predicates and notes that a previous test was not strictly correct and could classify an unrelated value such as `0x1a2a` as GREASE. The named macros encode the specification pattern once and make the intended value family visible at call sites.

**Implementation rule:** when a specification defines a value family as `base + k*step` or an equivalent modulo relation, first enforce the valid domain (`x >= base`, range, width, etc.) before subtracting or applying modulo in unsigned arithmetic. Do not depend on C wraparound to preserve the mathematical predicate.

**Review rule:** prefer a named predicate for specification-defined sentinel/reserved/grease families when the same rule is used in multiple places. Review both the formula and its input domain, and include boundary values just below the base as well as valid members in tests or manual validation.

**Confidence:** Very high. Merged master fix with direct John Thacker review and corresponding maintained-branch work; the release-3.6 cleanup independently documents a concrete false-positive risk from the older ad-hoc predicate.
