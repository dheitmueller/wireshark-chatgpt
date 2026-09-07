# Wireshark ChatGPT Engineering Notebook

This repository is a persistent engineering knowledge base used to make future Wireshark development work more accurate, idiomatic, and consistent with upstream conventions.

The goal is not to mirror the Wireshark source tree or preserve chat transcripts. Instead, this notebook captures durable, high-value knowledge that would otherwise be expensive to rediscover across conversations: architectural patterns, current APIs and conventions, reviewer preferences, good exemplars, testing/fuzzing practices, design decisions, known pitfalls, and the state of ongoing work.

## Instructions for ChatGPT

When the user asks ChatGPT in any conversation to read, review, consult, or use this notebook/repository for Wireshark work, that request implicitly includes permission and an instruction to **maintain and update the notebook with durable new findings discovered during that work**. The user should not need to separately say "update the notebook" each time.

Updates should be selective rather than transcript-like. Preserve knowledge that will improve future Wireshark development: verified conventions, architectural understanding, authoritative maintainer guidance, useful exemplars, mistakes/pitfalls, testing practices, design decisions and their rationale, and current unfinished work. Correct or remove stale information when new evidence supersedes it.

Before reviewing Wireshark merge requests, consult `reviewed-mrs.md` and avoid re-reviewing MRs already recorded there unless there is a specific reason to revisit one (for example, new review comments, substantial new commits, a changed outcome, or an explicit user request). After a substantive MR review, record it in `reviewed-mrs.md`, including enough status/context to know what was actually examined.

### Model-quality requirement for MR analysis

MR mining is intended to build a high-confidence long-term knowledge base, so do not knowingly perform it using a fallback/downgraded model caused by exhaustion of the user's normal higher-capability usage allowance. If the runtime/product explicitly indicates that the requested MR analysis is being downgraded or routed to an older/lower-capability fallback because of usage limits, **abort the MR analysis rather than updating the notebook with lower-quality conclusions**. Tell the user that the run was stopped because the preferred model was unavailable. Do not claim to detect a downgrade unless the runtime actually exposes that information; model routing may not always be visible to the assistant.

### Weight evidence by MR outcome

When deriving durable conventions from MR history, give substantially greater evidentiary weight to changes that were ultimately **merged**. A merged MR demonstrates that the resulting implementation/review resolution was accepted upstream. Open MRs can provide useful provisional evidence, especially authoritative maintainer comments, but their conclusions may still change. MRs that were abandoned, closed without merge, or superseded should normally carry less weight as examples of accepted implementation practice.

Do not discard review comments from abandoned/superseded MRs: an authoritative maintainer correction can still be valuable evidence about what *not* to do. Preserve the distinction between reviewer guidance and accepted final implementation. When possible, follow a superseded MR to its replacement and use the merged successor as the stronger implementation exemplar.

## Merge-request corpus

The raw Wireshark GitLab merge-request corpus is maintained separately at `dheitmueller/wireshark-corpus-mrs` on GitHub. Treat that repository as the preferred source for historical MR metadata, diffs, commits, diff versions, and review discussions instead of repeatedly retrieving the same information from GitLab.

Each MR is stored as `mr_<iid>.json`. When mining MRs:

1. Consult `reviewed-mrs.md` first.
2. Use the corpus repository to identify/read candidate MRs.
3. Prefer high-information-density MRs: substantive human review, core maintainer participation (especially Guy Harris), dissector/libwireshark changes, malformed/truncation handling, field/filter semantics, registration/handoff, reassembly, RTP/media, testing, and fuzzing. Among otherwise comparable candidates, prioritize merged MRs over open, abandoned, closed-unmerged, or superseded MRs.
4. Ignore GitLab system notes when assessing review value; prioritize notes with `system: false` and especially `DiffNote` discussions with position metadata.
5. After analysis, update the appropriate notebook topic files and the `reviewed-mrs.md` ledger.
6. Revisit a previously reviewed MR only when its corpus data shows meaningful changes (new head SHA, new human discussion, changed resolution/merge state) or the user explicitly asks.

The corpus is raw evidence; this notebook is the curated durable knowledge derived from it.

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
- `reviewed-mrs.md` — ledger of MRs already examined, used to avoid duplicate work.
- `decisions.md` — important design choices, rejected approaches, and rationale.
- `SESSION_STATE.md` — concise current work state and next steps.

## Maintenance

This notebook should evolve continuously while development work is in progress. Durable discoveries should be consolidated into topic files; temporary implementation state should stay in `SESSION_STATE.md`.
