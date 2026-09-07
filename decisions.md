# Engineering Decisions

## Persistent knowledge strategy

The notebook repository is the durable cache for project-specific Wireshark knowledge across ChatGPT conversations. It is not a replacement for the upstream source tree.

Decision: treat upstream Wireshark source as authoritative and use this repository to preserve expensive-to-rediscover understanding, conventions, reviewer feedback, and rationale.

Reason: conversational memory is summarized and does not reliably preserve detailed codebase understanding across long sessions or chat boundaries.

## Curation policy

Decision: do not store chat transcripts. Consolidate stable knowledge into topical files and keep temporary work state in `SESSION_STATE.md`.

Decision: when a conclusion comes from reviewer feedback, preserve provenance and confidence rather than presenting every comment as a universal rule.

## GitLab interaction

Decision: use Wireshark's public GitLab merge requests as read-only learning material. Do not post review comments, approvals, or otherwise interact with upstream MRs on the developer's behalf.
