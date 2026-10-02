# Merge-request dependency conventions

Prefer one coherent change per MR and keep MRs independently mergeable where practical. If change B genuinely depends on change A, hold or rebase B after A lands rather than stacking overlapping histories that can be accidentally squashed together.

The !2049/!2050 sequence demonstrates the failure mode: one MR was based on the other's unmerged changes, and squash merge caused both logical changes to land under the wrong commit identity. Anders Broman described the established workflow as one MR/one commit, with MRs independent or dependent work held until the prerequisite is committed.

Submit from a named topic branch rather than a protected fork `master` when the latter prevents maintainers from rebasing or making minor edits. Closed !2041 contains direct guidance from Guy Harris, Alexis La Goutte, and Pascal Quantin; the corrected work merged as !2051.

Evidence: merged !2049/!2050 and closed/superseded !2041, with the closed MR used only for workflow guidance.
