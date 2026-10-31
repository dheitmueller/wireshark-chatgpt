# Wireshark Display-Filter Type-Inference Conventions

This file records durable display-filter type-inference conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Infer comparison types from semantic counterparts, not operand position

A display-filter comparison should not become invalid merely because the operand that already carries an explicit field type appears on the right rather than the left. Constant expressions and literals may need contextual typing, and the compiler should use the typed counterpart when one side does not immediately determine its own type.

Merged master MR !12643 (`dfilter: Improve constant values type inferrence`) changes display-filter semantic analysis to look ahead to the opposite operand when a constant value's type cannot be inferred locally. This makes expressions such as `1 == frame.number` type-check consistently with the corresponding field-first spelling instead of requiring the typed field to occupy a privileged operand position. The accepted change also simplifies several type/error paths by doing that inference before later comparison handling.

**Implementation rule:** for symmetric binary operations such as equality, derive an otherwise-untyped literal or constant expression from the semantic type of its counterpart when the language permits that conversion. Do not encode operand-order accidents into the type system. If neither operand supplies enough type information, report the ordinary ambiguity/type error rather than guessing.

**Testing rule:** exercise equivalent comparisons in both operand orders, including constant expressions rather than only bare literals, and verify that invalid cross-type comparisons still produce normal user-facing diagnostics.

**Confidence:** Very high. Merged master display-filter compiler correctness change with a concrete previously rejected operand ordering.

## Resolve untyped function arguments from a definite type, not their position

Merged master !9216 changes `min()`/`max()` semantic checking so leading literal arguments need not be rejected before a later typed field supplies a type. The compiler defers resolving untyped literal nodes, finds a definite argument or expected type, then converts the pending literals; if no type can be established, it must not guess. Its temporary `dfw_scope` makes allocation cleanup possible even when filter compilation uses exceptions. Merged !9225 subsequently replaces strict equality of operand field-type enums with `compatible_ftypes()`, allowing the function to compare values whose types are semantically compatible. Merged !9220 prevents a preceding crash by validating node kinds before invoking field-only accessors; !9221 is its stable backport.

**Type-checking rule:** validate AST node kind before dereferencing type-specific storage; infer missing literal types from actual constraints; check semantic compatibility instead of equality of implementation enum labels; and provide ordinary user diagnostics for unresolved or invalid expressions. Test literal-first, typed-first, all-literal, incompatible-type, and malformed expressions.

Merged !9212 explored operand commutation for arithmetic inference. Guy Harris specifically questioned anti-commutative operators such as subtraction. João Valverde explained that the expression's AST operand order was preserved, **but later withdrew part of this approach in !9246**. Treat !9212 as instructive historical design/review evidence, **not** the current implementation to copy. **Confidence:** Very high for the merged crash and type-checking fixes; superseded for the commutation mechanism.
