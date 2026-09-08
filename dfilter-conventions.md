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