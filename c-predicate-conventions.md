# Wireshark C Predicate Conventions

This file records durable conventions for truth-valued C APIs and macros. Current upstream source and the relevant standards remain authoritative.

## Treat predicate results as zero versus nonzero unless exact values are promised

Merged master MR !9436 fixes several MinGW warnings and prompted direct review from Guy Harris. Code tested `isinf(x) == 1 || isinf(x) == -1`. Guy pointed out that the C standard only requires `isinf` to return nonzero if and only if the argument is infinite; an implementation may use another nonzero value. The accepted code simply uses `if (isinf(x))`.

**Implementation rule:** for predicate contracts expressed as zero versus nonzero, consume the result as a truth value. Do not infer exact true values from one libc, compiler, or platform unless the API explicitly promises them.

**Review rule:** boolean-comparison warnings can expose portable-C correctness bugs rather than merely compiler-specific style issues. Check the documented return domain before suppressing or rewriting a warning.

**Confidence:** Extremely high. Merged master fix with an explicit standards-based explanation from Guy Harris.
