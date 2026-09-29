# Wireshark Display-Filter Language Evolution Conventions

## Carry operator and precedence changes through every language layer

The merged arithmetic/language series !6562, !6568, !6575, !6577, !6598, and !6608 shows the expected scope for display-filter syntax evolution. The changes span scanner tokenization, Lemon grammar and precedence, semantic type checking, ftype arithmetic capabilities, VM opcodes, documentation, release notes, and regression tests. !6598 makes logical AND bind tighter than OR. !6608 demonstrates that symbolic arithmetic must work without whitespace while preserving MAC, IPv4/IPv6, and CIDR literals.

User feedback on modulo in !6568 exposed an LHS-expression gap, and !6575 adds the missing semantic support and tests.

**Language rule:** a new operator or precedence rule is not complete when the parser merely accepts it. Update lexical boundaries, associativity/precedence, AST/semantic typing, VM/runtime behavior, diagnostics, documentation/release notes, and tests as one compatibility unit.

**Lexer testing rule:** include adjacent-token cases such as field/literal operators with no spaces and regression cases for punctuation-heavy literals that could be re-tokenized by the new operators.

**Confidence:** Very high. All core changes are merged and largely authored by João Valverde, with end-to-end tests and documentation in the accepted implementation.

## Value-producing operators must work symmetrically in expression position

Merged !6488 promotes display-filter bitwise AND from a boolean-only test into a typed value-producing expression across scanner, grammar, syntax tree, semantic validation, ftype operations, VM execution, documentation, release notes, and tests. Merged !6499 fixes the missing RHS-expression path.

**Language rule:** once an operator produces a value, treat it as an ordinary typed expression everywhere expressions are legal. Test both operand positions, all advertised operand types, edge bounds, and error paths; parser acceptance is only the first layer of correctness.

**Confidence:** Very high. Both core changes merged and were authored by João Valverde; user testing and static analysis exposed concrete follow-up gaps.


## Migrate syntax in stages and keep scanner, semantics, tests, and docs synchronized

Merged master MR 4862 first adds comma-separated set elements while retaining the historical whitespace separator; release-3.6 MR 4874 carries the compatible syntax forward. Merged master MR 4881 later removes the deprecated whitespace form and updates the scanner, grammar, tests, release notes, User's Guide, and shipped filter examples. MR 4871 separately audits user documentation for the surrounding language changes.

The same batch shows where invalid forms should fail. Merged master MR 4864 removes the scanner's arbitrary-character fallback and tightens punctuation-sensitive token patterns. Merged master MR 4880 adds an operator-specific semantic check for the left operand of `matches`, turning an assertion crash into a normal type error with regression coverage.

**Migration rule:** introduce and test replacement syntax before removing the legacy form; then remove it with explicit negative coverage and synchronized documentation and examples. Treat the filter language as a public compatibility surface spanning lexer, grammar, semantic checker, diagnostics, documentation, and tests.

**Failure-layer rule:** reject lexical impossibilities in the scanner and type or operator impossibilities during semantic checking rather than letting them reach runtime assertions.

**Confidence:** Very high. A sequence of merged master language changes by João Valverde with a maintained-branch compatibility step and regression tests.
