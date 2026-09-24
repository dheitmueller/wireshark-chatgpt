# Wireshark Post-Rebase CI Conventions

## Re-run CI after a substantial rebase instead of treating old validation as transferable

A pipeline result belongs to the exact revision it tested. A long-lived merge request that is rebased across a large amount of upstream churn can acquire duplicate declarations, API interactions, or other integration failures that were absent when the earlier pipeline ran. Skipping validation after such a rebase discards the main mechanism intended to expose those changes.

Merged MR !10856 provides a concrete negative example. Review found overlapping protocol-control work and differing `proto_disable_all()` semantics; after the change, a redundant declaration produced a warning-as-error build failure. João Valverde explicitly told Anders Broman that it was a bad idea to skip the pipeline for a two-month-old MR rebased across roughly 700 commits. John Thacker later pointed to !11624 for the corrective build/configuration work.

**Submission rule:** after a substantial rebase or refresh of a stale MR, run the project pipeline on the rebased revision before treating the change as ready. Do not infer that CI on the pre-rebase history transfers to the new diff, and do not bypass the pipeline merely because the feature itself was previously reviewed.

This complements the existing submission rule that a green pipeline validates only the code actually present in the current MR diff: first verify the current diff, then ensure that exact revision receives the expected CI coverage.

**Confidence:** High. The warning is direct maintainer feedback on a merged MR, and the concrete integration failure demonstrates why stale pre-rebase validation was insufficient.
