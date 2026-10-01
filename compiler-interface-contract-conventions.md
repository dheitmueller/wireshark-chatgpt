# Compiler interface-contract conventions

## New compiler diagnostics can expose old C contract defects

A declaration/definition mismatch is a source-level contract problem even if older compilers accepted it. In merged !2324, GCC 11 diagnostics exposed inconsistent array parameter bounds. Guy Harris states that true mismatches should be fixed even when an alpha or beta compiler is what reveals them. Peter Wu adds the complementary review rule: verify that any declared bounds reflect actual callers before removing or changing them.

**Implementation rule:** classify the diagnostic semantically. Make declarations and definitions agree, but validate the intended API bounds before weakening a prototype merely to silence the warning.

**Confidence:** Very high. Merged master fix with direct Guy Harris and Peter Wu review.
