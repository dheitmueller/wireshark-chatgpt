# Wireshark ChatGPT Engineering Notebook

This repository is a persistent engineering knowledge base used to make future Wireshark development work more accurate, idiomatic, and consistent with upstream conventions.

The goal is not to mirror the Wireshark source tree or preserve chat transcripts. Instead, this notebook captures durable, high-value knowledge that would otherwise be expensive to rediscover across conversations: architectural patterns, current APIs and conventions, reviewer preferences, good exemplars, testing/fuzzing practices, design decisions, known pitfalls, and the state of ongoing work.

## Working principles

- Treat the current Wireshark source tree as authoritative.
- Prefer conclusions supported by current upstream code and/or repeated maintainer review feedback.
- Record provenance when a convention is inferred from merge requests or reviewer comments.
- Distinguish durable knowledge from temporary session state.
- Update or remove stale conclusions when upstream changes.
- Avoid transcript dumps; curate for future usefulness.
- When implementing new functionality, first find and study analogous upstream implementations.

## Files

- `architecture.md` — architectural notes and subsystem relationships.
- `dissector-conventions.md` — idioms, API usage, naming, registration, tree construction, and common review expectations.
- `media-over-ip.md` — SMPTE/RTP/media-specific notes, including ST 2110, ST 2038, and ANC work.
- `testing-fuzzing.md` — build, validation, fuzzing, and test practices.
- `review-patterns.md` — conventions learned from upstream merge request reviews.
- `decisions.md` — important design choices, rejected approaches, and rationale.
- `SESSION_STATE.md` — concise current work state and next steps.

## Maintenance

This notebook should evolve continuously while development work is in progress. Durable discoveries should be consolidated into topic files; temporary implementation state should stay in `SESSION_STATE.md`.
