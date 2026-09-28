# Review findings: 6261–6310

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

All 50 MRs in the exact run ledger were examined. The batch contains 45 merged submissions and five closed/unmerged submissions.

Highest-value evidence: 6299 (Guy Harris on exact Npcap runtime identity), 6279 (Guy Harris on macOS SDK/deployment-target/runtime availability), 6265 (Gerald Combs on preserving alignment safety), 6262 and 6277 (generated-source fixes belong in the generator/template and regenerated output), 6275 (post-merge 32-bit pointer/integer portability regression), 6272 (avoid redundant derived arguments), 6285 and 6288 (display-filter literal changes require parser/docs/tests), 6276 (multi-transport/reassembly capture tests), and 6266 (preserve provenance for manually supplemented generated data).

Durable conclusions: keep SDK choice separate from deployment-target guarantees; make runtime probes test the exact implementation they claim; keep generated fixes in the generator; use alignment-safe copying; do not encode integers through pointer storage across architectures; avoid redundant derived API arguments; and test language/transport changes across their complete semantic matrix.

Closed MRs 6303, 6298, 6283, 6270, and 6269 are weighted only as review history.
