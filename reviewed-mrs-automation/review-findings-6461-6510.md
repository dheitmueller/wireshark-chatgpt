# Wireshark MR review findings !6461-!6510

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Strong reusable findings:
- !6501: JSON object-member order must not become a parser requirement; locate fields needed for diagnostics explicitly.
- !6488 and !6499: value-producing display-filter operators must propagate through grammar, typed semantics, VM/runtime, docs, and tests, including both operand positions.
- !6494: sibling frontends should share a neutral application layer and initialize product-specific configuration identity centrally; ordinary legacy callers must preserve the default path.
- !6493: relaxing message syntax must not accidentally change long-lived stream framing or require EOF; this merged implementation has post-merge compatibility concerns and is cautionary evidence.
- !6486: Jaap Keuter requested explicit `static_cast` for Qt size narrowing; keep `qsizetype` until an API boundary that truly requires `int`.
- !6465: Jaap Keuter required the adjacent comment to be updated when the code's semantic category broadened.
- !6462: Gerald Combs validated a CMake dependency fix by mutating an input and rebuilding the narrow target; incremental rebuild behavior is the test that matters.
- !6461: decode structurally understandable nonconformance for visibility, but diagnose the protocol violation explicitly; later !6522 remains stronger taxonomy guidance.
- !6509 was merged but later reproduced as still broken and reverted by !6545, so it is negative/superseded evidence.

Closed !6489 and !6463 were down-weighted; !6463 explicitly yielded to merged !6471.
