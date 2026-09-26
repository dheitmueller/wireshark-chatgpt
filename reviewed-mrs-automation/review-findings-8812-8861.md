# Wireshark MR Review Findings: 8812-8861

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

Reviewed exactly 50 MRs, !8861 through !8812. Forty-nine merged; !8830 closed without merge.

Strong findings include the !8836 reassembly head/item type split and its immediate correctness follow-ups; !8847's rule that the common reassembly helper's returned TVBuff is the authority for layered reassembly availability; !8837's Qt model optimization with an explicit current-packet lifetime, challenged directly by Guy Harris; !8828's balanced recursion-depth accounting; !8826's separation of resolved custom-column display text from unresolved value text; !8852's before/after tshark-output comparison across seven PCEP captures for a behavior-preserving refactor; !8820's first-pass sequence-state persistence and protocol-transition boundary; !8853/!8829's distinction between compiler-warning policy and runtime-analysis job goals; !8843's sibling audit of invalid denominator calculations; and !8860's explicit dependency-header include.

New focused notebook files created by this run:
- reassembly-api-conventions.md
- custom-column-conventions.md
- refactor-validation-conventions.md

The exact reviewed set is in reviewed-mrs-automation/reviewed-mrs-automation-8812-8861.md.
