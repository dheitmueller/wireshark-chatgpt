# Durable conventions extracted from !6711-!6760

Corpus commit ddcaa22b51c68f594e425a23388c3a2086813054.

## Model language-wide modifiers as semantic qualifiers, not duplicated operator grammars

Merged !6760, authored and merged by Joao Valverde, adds display-filter any and all universal quantifiers across existing relational operators. The accepted implementation carries a match mode in the test AST and lets code generation select the corresponding VM opcode instead of cloning every grammar production for every operator. Documentation, release notes, and syntax tests move with the language change.

Rule: when one modifier changes the semantics of a whole operator family, represent it orthogonally in the AST/compiler where practical. Keep lexer, grammar, semantic checking, VM/codegen, user documentation, and regression tests synchronized as one language interface.

## Distinguish absolute stack depth from per-protocol occurrence identity

Merged !6759, authored by Joao Valverde, adds display-filter syntax for selecting a particular occurrence of a protocol field in nested encapsulation. To make that meaningful it introduces both total current layer depth and per-protocol occurrence depth, stores both on field_info, and tests repeated nested IP layers.

Rule: total dissector-stack position and the Nth occurrence of protocol X are different identities. State or user-visible selectors referring to repeated instances of one protocol should use protocol-relative occurrence identity; unrelated layers may shift absolute stack depth.

## Conversation proto-data APIs require a real conversation and should report contract violations centrally

Merged !6748, authored by Gerald Combs, documents that conversation add/get/delete proto-data calls require a non-NULL conversation and uses REPORT_DISSECTOR_BUG for violations. The discussion on merged !6755 is especially useful: silently tolerating a NULL pointer hides a programming error, while crashing deep in a release dissector can unnecessarily turn a diagnosable contract violation into a security/CVE incident.

Rule: shared conversation APIs should enforce impossible caller states at the abstraction boundary with Wireshark's dissector-bug reporting mechanism. A missing conversation is not equivalent to a valid conversation that simply has no protocol data. Callers should still establish semantically valid state rather than rely on defensive NULL tolerance.

## Tree-faking optimizations do not preserve mutable item identity

Merged !6746, authored by John Thacker, documents that when child items are faked by reusing another proto_item, later parent lookup or length mutation cannot determine which logical item the caller meant. This can push later additions to the root, defeat faking optimizations, and distort hierarchy statistics. Merged !6735, also authored by John, fixes hierarchy classification by querying proto_registrar_is_protocol() rather than inferring protocol-ness from a parent sentinel.

Rule: protocol-tree consumers should use semantic registry/type information rather than tree-shape accidents, display labels, or parent sentinel values. Optimizations that alias multiple logical items to one node are safe only when no later operation needs independent parentage, extent, or other mutable identity.

## Static checkers must account for every emitted warning and retire unsound checks

Merged !6732, authored and merged by Martin Mathieson, makes all real typed-item warning paths increment the checker warning count and removes the consecutive-mask warning that was not reliable enough. This is historical tooling evidence; the repository's current checker flags remain authoritative.

Rule: if a checker diagnostic is intended to influence CI/pre-submit status, every path that emits it must update the checker result consistently. When a heuristic rule is too noisy or semantically unsound, narrow or remove that rule rather than printing warnings that are not trustworthy.

## Lexer wrappers must propagate conversion failure tokens

Merged !6713, authored and merged by Joao Valverde, fixes quoted-string scanning by returning the token produced by set_lval_quoted_string() / set_lval_charconst() instead of unconditionally returning a successful STRING/CHARCONST token. The old wrapper discarded SCAN_FAILED.

Rule: when a lexer helper both constructs a semantic value and reports token/error status, the wrapper must propagate that status. Do not overwrite a validation/conversion failure with an unconditional success token merely because lexical delimiters matched.

## Plugin-extensible registration should be idempotent and encode precedence explicitly

Merged !6711, authored and merged by Gerald Combs, replaces the fixed conversation-filter protocol list with dynamic registration so plugins can participate. Roland Knall's review led to one centralized add function, duplicate suppression inside that function, and comments explaining ordering: lower layers are registered first because prepend semantics give later upper layers higher priority.

Rule: initialization-time registries that plugins/extensions may call should normally tolerate duplicate registration when repeated initialization is harmless. Centralize insertion and duplicate policy in one API, and document ordering when registry order affects dispatch/selection precedence.

## Treat display-filter abbreviations as compatibility-facing API

Merged !6716 changed IEEE 802.11 filter names while adding KDE support. CI immediately exposed stale tests, and Richard Sharpe explicitly questioned whether the rename would break user scripts.

Rule: an hf_ filter abbreviation is not merely a label. Before renaming it, search tests, docs, scripts/examples, and compatibility expectations; preserve the old name or deliberately document the compatibility change where practical.

## Corroborating evidence retained without duplicate rules

- !6734 adds frame-aware PDCP-NR security-key/configuration state and reset-boundary handling, reinforcing existing redissection-safe state-history rules.
- !6741 reinforces the submission rule that CI validates the actual commit message: amend and push the commit rather than changing only the MR title.
- !6715 again shows the normal reviewer expectation that new dissector behavior include a representative capture when feasible.
- !6754/!6753 keep support-matrix CI changes and release-note communication aligned.
- !6722 shows that expanding a generic ftype operation surface should include edge tests for each participating value domain, including signed/sub-second time representations.
