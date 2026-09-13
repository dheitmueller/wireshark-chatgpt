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