# Wireshark Display-Filter Conventions

This file records durable display-filter implementation conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Cache semantic qualifiers, not just field identity

When display-filter compilation caches or reuses field references, the cache key must include every qualifier that changes the reference's meaning. A field's registered `hf_` identity alone is insufficient if syntax such as layer selection, occurrence selection, slicing, or similar modifiers changes which values the expression denotes.

Merged MR !26305, authored and merged by John Thacker, fixes a display-filter compiler bug where multiple references to the same field with different layer selectors could incorrectly share one cached load. The accepted implementation makes the field-reference cache account for layer selection rather than treating all references to the same `hf_` as equivalent.

**Implementation rule:** before memoizing a parsed/compiled field expression, enumerate all syntax that affects its semantic value and include that state in the cache identity. Do not key only on the base field when two syntactically distinct references can legitimately resolve to different values.

**Confidence:** Very high. Merged master compiler correctness fix authored and merged by John Thacker.

## Reject recursive display-filter macro expansion explicitly

Display-filter macros are effectively a small expansion language. A macro cycle must be diagnosed as such rather than allowed to recurse until a generic depth, stack, or parser failure occurs.

Merged MR !26318, authored and merged by John Thacker, adds explicit recursion detection while expanding display-filter macros. The implementation tracks the active expansion chain and reports a cycle as a display-filter error instead of relying on incidental resource exhaustion.

**Implementation rule:** expansion engines should track the active dependency chain and reject re-entry into an already-active macro. A global maximum depth can remain a defense-in-depth bound, but it is not a substitute for cycle detection because acyclic deep expansion and recursive expansion are different error conditions.

**Confidence:** Very high. Merged master parser hardening authored and merged by John Thacker.

## Parse macro arguments at the language level, not by raw delimiter splitting

Display-filter macro arguments can themselves contain language constructs whose commas or parentheses do not delimit the outer macro invocation. Argument extraction therefore has to understand nesting and quoting rather than treating the argument text as a flat comma-separated string.

Merged MR !26306, authored and merged by John Thacker, fixes display-filter macro arguments containing nested function calls and other parenthesized expressions by parsing delimiters with nesting awareness.

**Implementation rule:** when a feature embeds display-filter expressions inside another syntactic form, delimit those expressions using the filter language's token/nesting rules. Do not split on punctuation without tracking grouping and quoted/string contexts.

**Confidence:** Very high. Merged master parser correctness fix authored and merged by John Thacker.

## Keep user-facing language documentation synchronized with parser capability

When a display-filter syntax feature is added or broadened, update user-facing reference material in the same development stream so users do not have to infer the accepted grammar from implementation details.

Merged MR !26314 adds documentation for arithmetic expressions and related display-filter behavior after the parser/compiler gained those capabilities. This is primarily documentation work, but it reinforces that the filter language is a user-facing interface whose documented grammar should match what the parser accepts.

**Submission rule:** parser or semantic-language changes should include corresponding documentation changes when they expose new syntax or meaning to users. Conversely, documentation examples should be exercised against the current parser where practical.

**Confidence:** High. Merged master documentation aligned with recently added filter-language features.

## Lexer keyword recognition must respect identifier boundaries

A keyword that appears as a prefix or substring of an identifier must not be tokenized as the keyword unless the language grammar actually permits that split. Lexers should recognize the complete token before applying keyword classification.

Merged MR !25845, authored and merged by John Thacker, fixes display-filter identifiers whose leading text matched a keyword. The accepted lexer behavior classifies the whole identifier rather than prematurely consuming a keyword prefix.

**Implementation rule:** tokenize the full identifier according to identifier syntax, then decide whether that complete token is a reserved word. Avoid regex/order arrangements where a keyword rule can consume the beginning of an otherwise valid identifier.

**Confidence:** Very high. Merged master lexer correctness fix authored and merged by John Thacker.

## Every parser-admitted expression must fail semantically, not internally

If the parser can legitimately construct an AST for an expression, the semantic checker/compiler must either compile it or reject it with a normal user-facing display-filter diagnostic. A syntactically valid but type-invalid expression must not fall through to an internal assertion, unreachable branch, or fatal DFilter error.

Merged master MR !14789, authored and merged by John Thacker, fixes arithmetic expressions involving string literals. The grammar could produce the expression, but type inference rejected it in a path that ultimately reached an internal assertion/fatal error. The accepted change makes the semantic layer diagnose the incompatible operand types cleanly.

**Implementation rule:** treat the parser's AST space as an input contract for the semantic checker. For each expression node/form the parser can emit, ensure type checking has an explicit valid-result or ordinary-error path. Negative tests should include legal syntax with illegal operand-type combinations, not just malformed syntax.

**Confidence:** Very high. Merged master compiler robustness fix authored and merged by John Thacker.

## Display flags are composable when registration permits combinations

Do not assume mutually exclusive display modes when the field-registration API permits flag combinations. Semantic analysis and execution must interpret every supported combination consistently.

Merged master MR !14506, authored and merged by John Thacker, fixes `BASE_VAL64_STRING | BASE_EXT_STRING`. The display-filter code had treated the extended-string flag as though it implied the 32-bit value-string representation, causing incorrect value-name resolution and matching for a legitimate combined 64-bit extended table. The accepted fix handles the combined representation in both semantic checking and execution, with IAX2 providing a concrete regression case.

**Implementation rule:** when `hfinfo->display` is a bitfield, review logic for relevant flag combinations rather than implementing a chain that assumes only one flag can be active. Type/representation selection in the semantic checker and runtime must agree.

**Confidence:** Very high. Merged master display-filter correctness fix authored and merged by John Thacker.

## Preserve absence semantics when internal value containers change

Display-filter functions can receive arguments that represent an absent field. That absence must remain semantically “no value”; it must not be coerced to zero merely because an internal container is missing, nor may consumers blindly dereference a concrete-container pointer. Refactoring the value representation can invalidate old assumptions such as “a NULL pointer is a valid empty list.”

Merged master MR !14287, authored and merged by John Thacker, fixes `min()`/`max()` after display-filter function values moved from `GSList` semantics to `GPtrArray`. Registers for expressions involving nonexistent fields can legitimately contain a NULL array. The accepted code ignores those NULL arguments for min/max, avoids unreferencing NULL during stack cleanup, and adds regression tests proving that a missing `udp.payload` remains NULL even through expressions such as `len(udp.payload[2:]) + 2` rather than becoming the numeric value 2.

**Implementation rule:** define absence/nullability at the VM/function interface independently of the concrete container representation. When changing representation, audit every producer, consumer, and cleanup path for the old representation's implicit empty-value behavior.

**Testing rule:** exercise absent fields through nested functions and intervening arithmetic/slicing operations, not only direct field loads, so tests cover registers that were never materialized into a concrete value container.

**Confidence:** Very high. Merged master semantic and crash fix authored and merged by John Thacker with targeted regression tests.