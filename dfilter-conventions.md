# Wireshark Display-Filter Compiler Conventions

This file records durable display-filter compiler and macro-processing conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Cache or register reuse is valid only when the semantic qualifiers are part of the identity

Compiler optimizations may reuse a previously computed field value only when the two computations are semantically equivalent. A shared `hfinfo` identifier is not sufficient if one query is range-limited, raw, transformed, or otherwise qualified differently.

Merged MR !26305, authored and merged by John Thacker, fixes DFVM register reuse for range-limited field references. Wireshark already avoided consuming an ordinary cached field register for a range-limited query; the missing inverse guard allowed a range-limited result to be stored and later reused for another form of the same field. The accepted fix simply declines that reuse and adds tests for range-limited and raw queries. Release backports !26307 and !26308 preserve the correction.

**Implementation rule:** an optimization cache key must encode every qualifier that can change the computed value. If doing so is disproportionate to the benefit, prefer not to cache/reuse that case rather than weakening semantic identity for a rare optimization.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker and propagated to two stable branches.

## Prevent recursive macro cycles by expansion semantics, not only by a depth ceiling

A recursion-depth limit is a useful resource guard, but it should not be the mechanism that gives macro expansion its basic cycle semantics. While replacing a macro, mark that macro unavailable to nested replacement of its own replacement list, while still allowing a later occurrence at the same outer scanning level when the language semantics permit it.

Merged MR !26318, authored and merged by John Thacker, aligns dfilter macro rescanning with the C preprocessor model: a macro being replaced cannot be replaced again through a deeper nested expansion, which prevents direct and indirect cycles. The existing nesting limit remains a defense against legitimately deep chains, whose output can still grow exponentially, but it is no longer relied on to terminate cyclic definitions.

**Implementation rule:** distinguish semantic cycle prevention from resource limits. Track the active expansion set or equivalent state across nested replacement, and separately enforce a conservative maximum expansion depth/size for valid-but-pathological inputs.

**Confidence:** Very high. Merged master language-semantics fix authored and merged by John Thacker.

## Validate macro argument references when the macro is defined, not when expansion happens

Invalid argument references are syntax errors and should fail during macro checking/creation. Deferring them until a macro happens to be expanded turns malformed configuration into a latent crash path.

Merged MR !26306, authored and merged by John Thacker, rejects `$0` because dfilter macro arguments are one-indexed and rejects argument numbers that overflow an `int`. Both previously could survive definition and crash only when the macro was used.

**Implementation rule:** parse and range-check indexes, counts, and numeric references at definition/compile time whenever their validity is independent of runtime packet data. Store only representations that satisfy downstream indexing contracts.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker.

## Treat generated documentation as untrusted until checked against implementation semantics

Documentation generated or expanded with AI assistance can confidently attach the wrong meaning to terse internal names. Review comments against the actual opcode/parser behavior, not merely naming intuition.

Merged MR !26314, authored and merged by John Thacker, corrects inaccurate AI-generated Doxygen comments that claimed `_R` opcodes denoted raw references when they actually denoted layer ranges.

**Review rule:** AI assistance does not reduce the evidence required for comments or documentation. Verify semantic claims against the implementation, specification, tests, or authoritative maintainer knowledge before merging them.

**Confidence:** High. Merged corrective change authored and merged by John Thacker with the specific documentation failure identified.

## Keep lexer case rules and reserved-name validation in lockstep

Changing whether language keywords or operators are case-sensitive also changes the namespace that user-registered identifiers are allowed to occupy. Parser/lexer recognition and registration-time collision checks must use the same normalization rules, or an identifier can be accepted as ordinary during registration but later tokenized as a reserved operator.

Merged MR !25845, authored and merged by John Thacker, makes display-filter operators case-insensitive and updates the checks for registered filter names so reserved operator names are rejected case-insensitively as well. The change deliberately excludes the `\x`, `\u`, and `\U` escape introducers from case folding because their case is semantically significant, and it updates the tests alongside the lexer behavior.

**Implementation rule:** whenever token matching changes case or normalization semantics, audit every other layer that recognizes or reserves those tokens—identifier registration, validation, documentation, and tests. Preserve explicit syntax-level exceptions rather than applying global case folding to constructs whose spelling itself carries meaning.

**Confidence:** Extremely high. Merged display-filter language change authored and merged by John Thacker with matching lexer, registration, and test updates.

## Every parser-admitted expression must reach a normal semantic result or diagnostic

The display-filter parser can legitimately construct syntax-tree nodes whose eventual operation is semantically invalid. Those cases are user input errors, not internal invariants, and the semantic compiler must handle them without hitting fatal assertions merely because a particular node type cannot participate in the requested operation.

Merged master MR !14789, authored and merged by John Thacker, fixes a crash when arithmetic is attempted between string literals. `STTYPE_STRING` is a parser-reachable operand form; the accepted change routes it through the normal semantic conversion path so the compiler reports an ordinary error such as `FT_STRING cannot be added` instead of terminating on a DFilter internal error/assertion.

**Implementation rule:** audit semantic dispatch over the full set of AST node kinds the parser can emit at that position. Invalid type/operator combinations should fail through the ordinary compile-time diagnostic path; reserve assertions for states that user input truly cannot produce after successful parsing.

**Testing rule:** negative display-filter tests should include syntactically valid but semantically invalid combinations for literals, fields, function results, and other parser-supported operand forms, and should assert a diagnostic rather than merely absence of a successful compile.

**Confidence:** Very high. Merged master robustness fix authored and merged by John Thacker.

## Treat valid field-display flags as composable dimensions when the API permits combinations

A field can legally combine display/value-string traits. Code that interprets `hfinfo->display` must not accidentally model those traits as mutually exclusive if the registration API permits combinations such as a 64-bit value-string table that is also an extended string table. Both the string-to-value semantic checker and the value-to-string execution path must agree on the combined representation.

Merged master MR !14506, authored and merged by John Thacker, fixes display-filter matching for `BASE_VAL64_STRING | BASE_EXT_STRING`. The old branch ordering handled an extended table as the 32-bit `value_string_ext` form and therefore failed for valid 64-bit extended tables; the semantic checker likewise needed to unwrap `val64_string_ext` before searching by text. The accepted fix handles both flags together in both directions. John used the IAX2 Wiki sample as a concrete reproducer: `iax2.voice.codec == "GSM compression"` changes from an impossible-value error to a valid `FT_UINT64` comparison.

**Implementation rule:** when flag bits describe orthogonal properties, branch on their legal combinations rather than assuming a single winning flag. Audit all conversion paths that consume the metadata so compile-time name resolution and runtime formatting use the same representation.

**Testing rule:** use at least one real registered field for each nontrivial supported flag combination, and test both symbolic-to-numeric filter compilation and numeric-to-symbolic matching/formatting where applicable. A synthetic unit test of one helper can miss disagreement between semantic checking and execution.

**Confidence:** Very high. Merged master display-filter fix authored and merged by John Thacker with a concrete sample-capture/`dftest` reproducer.