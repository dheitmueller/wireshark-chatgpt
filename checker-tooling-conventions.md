# Wireshark Checker Tooling Conventions

This file records durable conventions for project checking tools and pre-submit automation extracted from accepted upstream Wireshark changes. Current upstream source and tooling remain authoritative.

## Make checkers diagnostic by default; make rewriting an explicit action

A checker used from pre-commit or review automation should not silently edit the developer's source tree as a side effect of checking it. Detection and transformation have different trust and review requirements: a diagnostic can conservatively flag a suspicious construct, while an automatic rewrite must know that the proposed replacement preserves the code's semantics.

Merged master MR !21492 introduced a checker for suspicious `proto_tree_add_*` API choices. During review, Martin Mathieson objected to the checker silently rewriting files from pre-commit and suggested separating check and rewrite behavior. Michael Mann explicitly agreed that the tool should flag problems but not actively try to fix them by default. The accepted revision keeps rewriting behind an explicit command-line option whose default is false.

The same review also rejected bundling a very large set of automatic replacements into the checker MR. Michael noted that the correct fix is not always a mechanical function substitution—for example, sometimes the better correction is to improve the `hf_` field definition with units. The cleanup changes were therefore separated from the checker itself.

**Implementation rule:** checking/lint tools should report and fail appropriately without mutating source by default. If a reliable fixer is useful, expose it as an explicit opt-in mode. Keep broad semantic cleanup separate from the checker implementation when flagged sites require human judgment rather than guaranteed source-to-source transformation.

**Confidence:** Very high. Direct review from Martin Mathieson and Michael Mann on a merged master MR, with the accepted implementation changed to follow this behavior.

## Convert mechanically recognizable API misuse into executable project policy

When Wireshark has a stable API-selection rule that can be recognized syntactically, encode it in repository tooling instead of relying on every reviewer to remember it. The checker may be conservative, but recurring mechanical mistakes should become cheap to detect before review.

Merged MR !21492 adds project-wide detection for inefficient or redundant uses of `proto_tree_add_uint`, `proto_tree_add_uint_format_value`, and `proto_tree_add_uint_format`: when the formatted label merely reproduces the registered field label, use the value-formatting form; when formatting is only the value itself, use the ordinary typed add helper. The MR was ultimately integrated with Wireshark's checking workflow after substantial maintainer review.

**Implementation rule:** when review repeatedly exposes a syntactically identifiable class of API misuse, prefer adding or extending a Wireshark checker and integrating it with the normal pre-submit workflow. Keep the checker focused on cases where the diagnosis is dependable; leave context-dependent fixes for review or an explicit fixer mode.

**Confidence:** High. Merged master tooling change with extensive review and explicit discussion of the boundary between safe detection and semantic replacement.

## Make semantic fixups auditable inside bulk mechanical refactors

A scripted migration can produce a mostly mechanical diff while still requiring a small number of human semantic decisions. Those hand edits carry more review risk than the generated replacements and should be easy to identify and review independently; a tree merely compiling after “fixups” is not evidence that those decisions are semantically correct.

During merged MR !16268, Martin Mathieson explicitly asked what manual fixes were needed after the type-conversion script. Stig Bjørlykke warned that a very large MR made those fixes hard to distinguish and that “fixes to make it compile” could be merged without checking whether they were correct. Merged MR !16265 illustrates a better review shape by explicitly calling out the exceptional manual change. Closed MR !16278 supplies only secondary negative evidence: a compiler issue exposed by conversion was better treated as an independent focused fix than as a reason to convert every dissector.

**Review/submission rule:** for large scripted refactors, make the mechanical transform reproducible and isolate or explicitly enumerate every manual semantic fix. Prefer separate commits or focused follow-up MRs when a hand change has its own correctness argument. Review dependency/API contracts for those edits; do not accept successful compilation as proof of semantic correctness.

**Confidence:** High. Direct maintainer review on a merged bulk refactor, corroborated by an adjacent merged example; the closed MR is used only as secondary evidence.

## Treat newly enabled warnings as bug-discovery signals, not merely cleanup work

Turning on a warning class in a mature generator or build path can expose real semantic bugs that had been hidden by permissive language behavior. Warning-enablement work should therefore inspect and understand each nontrivial diagnostic rather than mechanically suppressing it or casting it away.

Merged MR !16081 enables Perl warnings across PIDL and fixes several issues they expose. In particular, the warning cleanup reveals that hexadecimal range bounds represented as strings were being treated numerically as zero in generator logic; the accepted change adds explicit integer parsing rather than silencing the diagnostic. The same upstream-sync batch also fixes precedence and initialization problems found while making the code warning-clean. Adjacent merged MR !16080 fixes another concrete `undef`/empty-array problem needed for warning-clean execution.

**Review/tooling rule:** when enabling stricter compiler, interpreter, or static-analysis diagnostics, classify each warning by semantic cause. Prefer a correctness fix or a more precise representation over a cast/suppression that merely makes the warning disappear. A warning-enablement MR may legitimately discover functional defects; call those out explicitly so reviewers do not mistake them for mechanical noise.

**Confidence:** Very high. Merged John Thacker upstream-sync work where enabling warnings directly exposed and fixed a generator logic bug.