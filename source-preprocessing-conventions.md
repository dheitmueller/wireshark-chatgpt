# Wireshark Source-Preprocessing Conventions

This file records durable conventions for source-to-source preprocessing and parser/generator tooling extracted from accepted upstream Wireshark changes. Current upstream tooling remains authoritative.

## Preserve source coordinates when preprocessing parser input

A preprocessing pass that removes comments or otherwise normalizes source text must not silently destroy the source-coordinate relationship used by later lexer/parser diagnostics. If the transformed input is intended to retain original source locations, preserve line structure (and column structure where relevant), or maintain an explicit source map.

Merged master MR !12508 changes `asn2wrs` to remove C-style comments during the preparse stage. Martin Mathieson explicitly asked whether the transformation preserved line numbering; Anders Broman confirmed that preserving it was the intent and that he had tested that behavior. That review point matters beyond comments: syntax and semantic diagnostics produced after preprocessing are useful only if their reported locations still identify the corresponding original source.

**Implementation rule:** when stripping comments, directives, or other lexical material before a downstream parser sees the input, decide explicitly whether diagnostics are defined in original-source or transformed-source coordinates. For original-source coordinates, retain the removed material's newline structure and any other location information the parser depends on, or carry an explicit mapping.

**Testing rule:** include a case where a diagnostic occurs after a removed or rewritten region and verify that its reported source location still points to the intended location in the original input. Testing only whether the transformed text parses successfully does not validate diagnostic stability.

**Review rule:** when preprocessing logic is adapted from another implementation, also verify provenance and license compatibility. In !12508 Martin separately raised the provenance/license question for the borrowed comment-removal approach before the MR was merged.

**Confidence:** High. The source-location invariant was called out explicitly in review by Martin Mathieson and confirmed/tested by the maintainer of the merged master change.