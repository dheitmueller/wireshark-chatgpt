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

Merged master MR !12941 reinforces the same boundary from the opposite direction: a field reference such as `${@frame}` has the same sliceable `FT_BYTES` semantics as the corresponding ordinary field, even though it is represented by a distinct AST node type. The old semantic checker handled `STTYPE_FIELD`, functions, and nested slices but rejected `STTYPE_REFERENCE`; the accepted fix dispatches explicitly on the node kind, obtains the field type for both field and reference nodes, and adds a regression test for slicing a byte-valued reference.

**Implementation rule:** treat the parser's AST space as an input contract for the semantic checker. For each expression node/form the parser can emit, ensure type checking has an explicit valid-result or ordinary-error path. Distinct AST node kinds that denote the same value category should receive the same capability checks rather than inheriting restrictions from representation accidents. Negative tests should include legal syntax with illegal operand-type combinations, while positive tests should cover equivalent capabilities through references and other alternate AST forms.

**Confidence:** Very high. Both are merged master semantic/compiler correctness fixes; !12941 was authored and merged by João Valverde and includes a targeted regression test.

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

## Model language-change notifications by their semantic effect, not one implementation source

A UI or analysis signal that means existing display-filter text may have changed validity should describe and be emitted for that semantic condition, not be narrowly tied to whichever subsystem first needed the signal. Dynamic field registration, loaded dictionaries, display-filter macros, and future user-defined syntax can all change the set of valid expressions.

Merged master MR !14117, authored and merged by John Thacker, restores revalidation after display-filter macros are reloaded. During review, Guy Harris explicitly corrected the implementation-oriented description of the existing “fields changed” signal: dynamically generated fields do not all come from dissector UATs (RADIUS can generate them from FreeRADIUS files), and the real event is that something changed the syntax of valid display filters. That includes named fields, macros, and other user-defined syntactic items even when no dissector changes its registered fields.

**Architecture rule:** name and document invalidation events by what consumers must reconsider. Every source that can change display-filter syntax or validity should trigger the same semantic invalidation path, and consumers such as filter editors and live-capture logic should revalidate from that event rather than knowing every producer.

**Confidence:** Extremely high. Merged master lifecycle fix authored by John Thacker with direct architectural clarification from Guy Harris.

## Recompile filters at semantic invalidation boundaries, not merely because more packets arrived

A compiled display filter is not necessarily a pure function of its source text. Compilation can capture context such as macro definitions, hostname resolution results, and selected-frame field references. Recompiling unchanged text at the wrong lifecycle boundary can therefore change meaning or fail transiently even though the user did not request a semantic refresh.

Merged master MR !14134, authored by John Thacker, stops recompiling an already validated display filter when a live capture merely appends packets. The MR notes that name resolution can time out or change, macros can be edited, and field references depend on the selected frame. Explicit rescans, reloads, retaps, or opening another file still need compilation against the new context; tailing the same live capture should preserve the existing compiled filter and its context.

**Implementation rule:** define which lifecycle events invalidate compiled filter code. Preserve the compiled object across operations whose contract is only “process newly arrived packets,” and recompile when the operation intentionally establishes a new language or capture-analysis context.

**Confidence:** Very high. Merged master display-filter lifecycle correctness fix authored by John Thacker.

## Enforce lexical contracts before lookup and use one predicate across equivalent syntax forms

When an identifier has a defined character set, reject an invalid character at the lexical boundary instead of continuing to parse a larger invalid name and later reporting that no such object exists. If the language offers multiple syntactic spellings for the same identifier class, they should share one lexical predicate so their accepted names cannot drift apart.

Merged master MR !14143, authored and merged by John Thacker, changes both display-filter macro syntaxes to use the same macro-name character predicate. A `-` or `.` in a macro name now stops parsing as an invalid character instead of becoming part of a nonexistent macro name and producing a misleading lookup error. The permissive path was identified as legacy behavior left over from an older architecture in which field references passed through macro handling.

**Implementation rule:** keep token validity in the lexer/parser layer and semantic existence in the lookup layer. Consolidate the token predicate for equivalent syntax forms rather than maintaining parallel character tests.

**Confidence:** Very high. Merged master parser/diagnostic cleanup authored and merged by John Thacker.

## Preserve the semantic distinction between a missing argument and an empty-string value

A missing/null macro argument and a literal empty string are different language values. Converting the former into unquoted empty text defers the error into a later grammar stage, producing diagnostics about whatever tokens happen to become adjacent instead of explaining the actual invalid call.

Merged master MR !14124, authored by John Thacker and merged by Anders Broman, rejects null display-filter macro arguments directly while continuing to allow a quoted empty-string argument where the filter language permits one. It also treats the syntactic form of an empty argument list as zero arguments rather than one null argument.

**Implementation rule:** preserve absence, empty-list, and empty-string states until the language semantics decide among them. Diagnose an invalid missing argument at macro-call validation rather than substituting text and relying on a downstream parse failure.

**Confidence:** Very high. Merged master display-filter macro semantics fix authored by John Thacker and accepted by Anders Broman.

## New surface syntax must preserve or explicitly redefine macro semantics

Display-filter macros are textual expansion, even when an invocation spelling looks like an ordinary function call. A new syntax alias must not silently change precedence or grouping semantics merely because its surface form suggests function behavior; conversely, if the project wants function-like grouping, that is a language-semantics change and must be treated as such for compatibility.

Merged master MR !12960 adds `$name(arg1, ...)` as an alternate spelling for the historical `${name:arg1;...}` macro syntax and documents that macro expansion remains recursive textual replacement performed before filter compilation. Review discussion exposed the important consequence: negating a macro whose body contains `or` can surprise users because the macro body is not automatically parenthesized. The accepted MR kept the existing textual semantics, while discussion recognized that adding implicit parentheses would be a separate backward-incompatible language change rather than a harmless parser tweak.

**Implementation rule:** when adding alternate syntax for an existing language feature, preserve the existing semantic model unless the change explicitly proposes and tests a compatibility break. For textual macros, document that callers or macro definitions must supply grouping where precedence matters; do not infer new grouping merely from function-like punctuation.

**Review rule:** evaluate new language syntax not only for parseability but also for the semantic expectations its shape creates. Test precedence-sensitive examples, especially negation and boolean operators, before deciding whether an apparently ergonomic syntax is truly an alias or a new language construct.

**Confidence:** High. Merged master language feature by João Valverde with explicit maintainer discussion of textual-expansion semantics, precedence surprises, and backward compatibility.
## Keep decoded field semantics and raw packet-byte access as distinct value domains

A registered field normally denotes its decoded, typed semantic value. Decoding can legitimately normalize the original packet representation—for example, malformed text can become a replacement character—so the decoded value is not always a reversible representation of the source bytes. When users need to inspect the original encoding, expose that as an explicit raw-byte view instead of weakening or changing the ordinary field's type semantics.

Merged master MR !8660, authored and merged by João Valverde, adds the display-filter `@field` form for exactly this purpose. A normal string field remains an `FT_STRING` value, while `@field` is compiled and evaluated as `FT_BYTES` taken from the field's packet-backed source range. The implementation carries raw-ness explicitly through syntax-tree nodes, VM operands, field loads, references, and register caching, and the change includes user documentation and regression tests.

**Implementation rule:** treat raw packet representation as a separate semantic qualifier on a field reference. Do not make malformed-byte inspection depend on the decoded field value retaining information that its type is allowed to normalize or replace.

**Compiler rule:** any cache or reuse of field loads/references must distinguish raw and decoded access, just as it must distinguish layer or occurrence qualifiers. Two references to the same `hf_` identity are not equivalent when they request different value domains.

**Confidence:** Very high. Merged master display-filter feature authored and merged by João Valverde with end-to-end parser/VM/test/documentation support.

## Remove experimental syntax when it collides with established grammar

A newly introduced notation is not worth preserving merely because it is expressive if it makes existing expressions lexically ambiguous. Language evolution should protect established operators and leave syntax space unclaimed until a concrete requirement justifies reserving it.

Merged master MR !8444, authored and merged by João Valverde, removes the recently introduced angle-bracket generic-literal syntax after it made an ordinary relational expression such as `a < b or a > c` ambiguous. Rather than inventing another reserved delimiter immediately, the accepted change keeps the narrower colon syntax for byte arrays, documents the removal in the release notes and User's Guide, and adds a regression test for the relational-expression case. Stable backport !8431 preserves the correction.

**Language-design rule:** if experimental syntax conflicts with established grammar, prefer removing or narrowing the experimental form over complicating the lexer around an avoidable ambiguity. Do not reserve a new punctuation form speculatively when current requirements are already met by less ambiguous syntax.

**Testing rule:** parser fixes for ambiguity should include the concrete expression shape that was previously mis-tokenized, not only positive tests for the replacement syntax.

**Confidence:** Very high. Merged master parser-language correction by João Valverde with a targeted regression test and stable backport.

