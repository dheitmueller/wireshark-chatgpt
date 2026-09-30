# Wireshark Display-Filter Type-Inference Conventions

This file records durable display-filter type-inference conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Infer comparison types from semantic counterparts, not operand position

A display-filter comparison should not become invalid merely because the operand that already carries an explicit field type appears on the right rather than the left. Constant expressions and literals may need contextual typing, and the compiler should use the typed counterpart when one side does not immediately determine its own type.

Merged master MR !12643 (`dfilter: Improve constant values type inferrence`) changes display-filter semantic analysis to look ahead to the opposite operand when a constant value's type cannot be inferred locally. This makes expressions such as `1 == frame.number` type-check consistently with the corresponding field-first spelling instead of requiring the typed field to occupy a privileged operand position. The accepted change also simplifies several type/error paths by doing that inference before later comparison handling.

**Implementation rule:** for symmetric binary operations such as equality, derive an otherwise-untyped literal or constant expression from the semantic type of its counterpart when the language permits that conversion. Do not encode operand-order accidents into the type system. If neither operand supplies enough type information, report the ordinary ambiguity/type error rather than guessing.

**Testing rule:** exercise equivalent comparisons in both operand orders, including constant expressions rather than only bare literals, and verify that invalid cross-type comparisons still produce normal user-facing diagnostics.

**Confidence:** Very high. Merged master display-filter compiler correctness change with a concrete previously rejected operand ordering.


## Use the expected type before falling back to field-name resolution

Merged master MR !6631, authored by João Valverde, fixes an ambiguity where an unparsed RHS token such as `fc` could be captured as a registered protocol/field even when the LHS already established a bytes domain. Semantic checking now first tries conversion as the expected typed literal (and value-string value where applicable), and only falls back to registered-field resolution if that conversion does not produce a value. The regression suite separately checks the explicit field spelling `.fc`.

**Type-inference rule:** when an operation supplies a semantic type for an ambiguous token, try that typed interpretation before consulting the global field namespace. Do not let an unrelated registered abbreviation override a literal that is valid in the expected type domain.

**Testing rule:** cover both the context-typed literal and an explicit field/protocol spelling.

**Confidence:** Very high. Merged master display-filter compiler fix by João Valverde with targeted regression tests.

## Reinterpret ambiguous RHS identifiers through the expected value context before rejecting them as protocol names

Display-filter tokens can lexically match registered protocol abbreviations even when the surrounding operator expects a literal value. Resolving such a token permanently to the global protocol namespace too early can make otherwise meaningful RHS expressions fail for reasons unrelated to the operation's actual type domain.

Merged master MR !4214 first fixes byte strings such as `fc` that also name protocols. Merged master MR !4238 generalizes the approach: when such a protocol-name token appears on the RHS, semantic analysis converts it back to an unparsed token and lets the normal context-sensitive literal rules decide whether it is bytes, a string, or another admissible value. The source comments explicitly acknowledge remaining lexical ambiguity rather than treating the scanner's early classification as semantically final.

**Type-inference rule:** treat early identifier/protocol classification as revisable when the operator's semantic context excludes that interpretation. Feed the token through the ordinary expected-type conversion path instead of hard-coding one relation-specific substitute.

**Historical note:** this is an early precursor to the stronger merged !6631 rule above, which more generally gives expected typed-literal conversion priority over global field-name resolution.

**Confidence:** Very high. Both master fixes merged; !4238 is the accepted generalized version authored by John Thacker after João Valverde review.

