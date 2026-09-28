# Wireshark Display-Filter Language Evolution Conventions

## Carry operator and precedence changes through every language layer

The merged arithmetic/language series !6562, !6568, !6575, !6577, !6598, and !6608 shows the expected scope for display-filter syntax evolution. The changes span scanner tokenization, Lemon grammar and precedence, semantic type checking, ftype arithmetic capabilities, VM opcodes, documentation, release notes, and regression tests. !6598 makes logical AND bind tighter than OR. !6608 demonstrates that symbolic arithmetic must work without whitespace while preserving MAC, IPv4/IPv6, and CIDR literals.

User feedback on modulo in !6568 exposed an LHS-expression gap, and !6575 adds the missing semantic support and tests.

**Language rule:** a new operator or precedence rule is not complete when the parser merely accepts it. Update lexical boundaries, associativity/precedence, AST/semantic typing, VM/runtime behavior, diagnostics, documentation/release notes, and tests as one compatibility unit.

**Lexer testing rule:** include adjacent-token cases such as field/literal operators with no spaces and regression cases for punctuation-heavy literals that could be re-tokenized by the new operators.

**Confidence:** Very high. All core changes are merged and largely authored by João Valverde, with end-to-end tests and documentation in the accepted implementation.
