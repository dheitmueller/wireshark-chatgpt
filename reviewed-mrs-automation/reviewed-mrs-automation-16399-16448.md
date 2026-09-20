# Automated Wireshark MR review: !16399–!16448

Corpus commit reviewed: `130eea3c8a60902c0fc98c9e1130e73bafc6f815`

Selection method: enumerated the complete recursive Git tree for the corpus commit (`truncated: false`), rebuilt the already-reviewed MR set from `reviewed-mrs.md` plus all available files under `reviewed-mrs-automation/`, explicitly preserving/counting the historical !17571–!17620 batch, subtracted that exact set from corpus MR numbers, sorted descending, and selected the first 50. No numeric interval was assumed reviewed merely from a ledger filename or neighboring reviewed MRs.

Exact MRs reviewed in this run (descending):

!16448, !16447, !16446, !16445, !16444, !16443, !16442, !16441, !16440, !16439, !16438, !16437, !16436, !16435, !16434, !16433, !16432, !16431, !16430, !16429, !16428, !16427, !16426, !16425, !16424, !16423, !16422, !16421, !16420, !16419, !16418, !16417, !16416, !16415, !16414, !16413, !16412, !16411, !16410, !16409, !16408, !16407, !16406, !16405, !16404, !16403, !16402, !16401, !16400, !16399.

## Durable findings promoted to the notebook

- **!16441 (merged master, John Thacker; approved/merged by Anders Broman):** heuristic recognition should use tvbuff availability predicates and reject incomplete candidates rather than relying on unsigned offset arithmetic that can wrap and let a bounds exception escape. Added to `probe-control-flow-conventions.md`.
- **!16407 (merged master):** ordered Wiretap format probing has meaningful order and early-exit semantics; generic foreach/container abstractions are not automatically preferable when they obscure early termination or need side-channel bailout state. Added to `probe-control-flow-conventions.md`.
- **!16432 (merged master):** sorted-container insertion direction can be a major workload-sensitive performance property. The accepted API adds a clearly named tail-biased `wmem_list_append_sorted()` rather than an ambiguous abbreviation or mode boolean; a reproducing capture was supplied. Added to `sorted-container-search-conventions.md`.
- **!16418 (merged release-4.0, Gerald Combs; approved/merged by John Thacker):** invoke Wireshark tests through the supported CTest/test.py integration rather than assuming a Python suite is pytest-compatible. Added to `test-runner-integration-conventions.md`.

Notebook convention commits made before this ledger:

- `cc1a38622c7b27daaa4891bd3a29722cf17b6e20` — recognition-probe control flow.
- `8490d29113384058b3aec71f9254e664ff3fd3ef` — sorted-container search direction/API naming.
- `ecebd4c40eb1b6bb192dd8ea2af542b5460236f7` — supported test-runner entry point.

## Additional evidence retained but not promoted as a new convention

- **!16420**, authored and merged by Guy Harris, fixes the macOS install script to use the actual `$installation_prefix/bin` location for libtoolize. This is high-authority accepted evidence, but the lesson is a localized path-correctness fix rather than a new project-wide convention.
- **!16443**, authored by Gerald Combs and merged by Anders Broman, stages deprecation of GLib-style tvbuff integer accessors until internal dissector migration is complete, avoiding a period of widespread compiler noise. Useful migration evidence, but consistent with existing API-evolution guidance.
- **!16414, !16410, and !16409**, merged Qt fixes by John Thacker, reinforce the existing rule that modal/auxiliary dialogs should be correctly parented for cross-window-manager behavior.
- **!16422, !16426, !16403, and !16432** provide concrete capture-backed validation/performance examples, reinforcing existing capture-backed testing guidance.
- **!16416** and **!16405** were closed/unmerged and were therefore down-weighted relative to accepted implementations.

This ledger records exactly the 50 MRs reviewed in this run; it does not claim that adjacent MR numbers are reviewed.