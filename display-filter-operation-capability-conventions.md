# Wireshark Display-Filter Operation Capability Conventions

This file records durable conventions for validating display-filter operations against the semantic capabilities of field/value types. Current upstream display-filter type rules remain authoritative.

## Reject unsupported operator/type combinations during semantic checking

A display-filter expression should be rejected by the semantic checker when the operand type does not implement the requested operation. Do not let an invalid user expression reach constant folding or runtime evaluation code that assumes the operation exists.

Merged master MR !9161, authored and merged by João Valverde, fixes a crash caused by applying unary minus to a type that does not support negation. The accepted change asks the field-type layer whether unary minus is supported before evaluating or precomputing the expression and reports a normal semantic-check failure otherwise.

**Implementation rule:** use the type system's capability predicates before applying an operator whose support varies by type. The semantic checker should be the boundary that turns an invalid expression into a diagnostic rather than an evaluator failure.

**Review rule:** when adding an operator or a new field type, audit both the evaluation implementation and the semantic capability checks. Constant-folding paths require the same validation as runtime paths.

**Confidence:** Very high. Merged master crash fix authored and merged by João Valverde.
