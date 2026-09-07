# Upstream Merge Request Review Patterns

This file records reusable lessons learned from Wireshark GitLab merge-request reviews.

## Evidence standard

For each pattern, record:

- MR number/link or other provenance.
- Reviewer/maintainer feedback in paraphrased form.
- The resulting code change when known.
- Whether the lesson appears to be a one-off preference or a reusable convention.
- Confidence level based on corroboration from other MRs/current source.

Do not normally turn a single review comment into a universal rule without checking current source and, where practical, other reviews.

## Reviewer authority

### Guy Harris

Treat technical review feedback from Guy Harris as extremely authoritative for Wireshark architecture, packet-dissection semantics, capture-file behavior, protocol interpretation, portability, and long-standing project conventions. He is a key Wireshark maintainer with more than two decades of project experience. A clear technical correction from him should carry substantially more evidentiary weight than an ordinary one-off review comment.

This does not mean mechanically generalizing every context-specific comment into a universal rule. Preserve the context and rationale, then corroborate against current source where the scope is unclear. However, when his feedback identifies an architectural or API convention, assume it reflects intentional project practice unless current upstream code demonstrates otherwise.

## Patterns

No reviewed MRs recorded yet. Start with Wireshark MR !26390 and add only conclusions that can be directly supported by the actual diff/discussion.
