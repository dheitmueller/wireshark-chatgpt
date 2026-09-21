# Wireshark Shared-Language Parser Conventions

This file records durable conventions for features that expose display-filter or other existing Wireshark expression syntax in a new UI or configuration surface. Current parser/compiler behavior remains authoritative.

## Reuse the canonical parser/compiler instead of approximating a shared expression language

When a feature claims to accept display-filter expressions, it should use the display-filter parser/compiler rather than a parallel regex or ad-hoc subset parser. A second parser tends to diverge on grouping, functions, slices, operators, macros, and future language additions, and it creates inconsistent diagnostics across surfaces that users reasonably expect to behave alike.

Merged master MR !14330, authored by John Thacker and merged by Anders Broman, changes custom columns so they can use display-filter functions, slices, arithmetic, logical tests, raw byte addressing, the layer modifier, macros, and combinations of those forms. The implementation compiles the custom-column expression with the display-filter compiler instead of extending the previous regex-based field parsing. It explicitly documents context-dependent exceptions such as field references, for which a custom column has no notion of a currently selected frame, and deliberately preserves the historical multifield-column `X or Y` interpretation for backward compatibility.

**Implementation rule:** if two features expose the same expression language, share its lexer/parser/compiler and semantic diagnostics. Gate only the constructs that genuinely require unavailable runtime context, and make those limitations explicit rather than silently implementing a syntactic subset.

**Compatibility rule:** when replacing an older ad-hoc grammar with the canonical language parser, identify syntax whose historical meaning differs from the canonical language and preserve or migrate it deliberately. Do not let a parser upgrade silently reinterpret persisted user configuration.

**Review rule:** test nested/grouped expressions, functions, operators, macros, and malformed expressions through the new surface, plus any legacy syntax being preserved. The canonical parser removes one source of grammar drift, but the embedding feature still owns its context restrictions and compatibility semantics.

**Confidence:** Very high. Merged master architecture change authored by John Thacker and accepted by Anders Broman, with the compatibility and unsupported-context boundaries documented in the MR itself.
