# Wireshark Parser Phase-Separation Conventions

This file records durable parser and language-processing conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Preserve lexical ambiguity until semantic context can resolve it

When the same unquoted token shape can legitimately denote more than one language-level value, the lexer should not force a classification that requires semantic knowledge it does not yet have. Preserve an explicit unresolved/unparsed representation and resolve it in the semantic/type-checking phase, where symbol tables, expected operand types, operators, and surrounding expression context are available.

Merged master MR !13144, authored and merged by João Valverde, restores an `UNPARSED` display-filter syntax type after attempts to decide field-versus-literal identity at the lexical level created limitations and corner cases. The MR calls out tokens such as `fc`, which can be a protocol/field identifier or the byte value `0xfc`, and dotted text such as `aa.bb.cc`, which can likewise be ambiguous. The accepted implementation defers the decision to semantic checking rather than treating lexical lookup as authoritative.

**Implementation rule:** tokenize what the source text proves, not what a partial symbol lookup merely suggests. If classification depends on type or name-resolution context, carry an unresolved node/token forward and let semantic analysis decide. This keeps lexer behavior stable as the language grows and avoids misleading errors caused by premature classification.

**Diagnostic rule:** prefer the phase that has enough context to explain the real error. A semantic checker can distinguish an unknown field, an invalid literal, or a type mismatch more accurately than a lexer that guessed one interpretation early.

**Testing rule:** exercise ambiguous spellings in every supported semantic role, including forms that are valid as names, valid as literals, and invalid under the surrounding operator/type context. Parser tests should verify both acceptance and the resulting diagnostic, not just tokenization.

**Confidence:** Very high. Merged master display-filter parser architecture change authored and merged by João Valverde; the MR rationale explicitly explains why lexical resolution was insufficient.
