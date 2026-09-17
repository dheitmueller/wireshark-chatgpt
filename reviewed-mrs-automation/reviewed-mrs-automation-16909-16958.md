# Wireshark MR review automation: !16909–!16958

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Review direction: descending from newest toward older. Existing `reviewed-mrs.md` and all available `reviewed-mrs-automation/` ledgers were consulted before selecting this batch. The historical !17571–!17620 batch remains part of the already-reviewed set.

## Exact reviewed set

!16958, !16957, !16956, !16955, !16954, !16953, !16952, !16951, !16950, !16949, !16948, !16947, !16946, !16945, !16944, !16943, !16942, !16941, !16940, !16939, !16938, !16937, !16936, !16935, !16934, !16933, !16932, !16931, !16930, !16929, !16928, !16927, !16926, !16925, !16924, !16923, !16922, !16921, !16920, !16919, !16918, !16917, !16916, !16915, !16914, !16913, !16912, !16911, !16910, !16909

Count: 50.

## Review notes

Merged master changes were weighted above release backports and abandoned/superseded work. Human maintainer discussion was weighted according to authority and specificity; no batch item established a sufficiently new durable convention to justify modifying a convention file.

Notable evidence:

- !16958 (merged master, Ivan Nardi): adds TRDP Message Data over TCP using a dedicated TCP entry point and `tcp_dissect_pdus()`, with a minimum fixed header before reading the protocol payload length. Useful corroboration for stream-PDU framing and separate transport entry points.
- !16950 (merged master, Martin Mathieson): O-RAN FH CUS usability/display cleanup. Mostly protocol-specific presentation improvements; no new general rule.
- !16940 (merged release-4.4 cherry-pick): DoIP/UDS changes Info-column handling so multiple messages in one packet are appended rather than later messages overwriting earlier ones. Weighted below the originating master change.
- !16930 (merged master, Martin Mathieson): O-RAN FH CUS fuzz-error fix separates even/odd page or compression state and avoids treating initialized/default state as decoded protocol state. Corroborates existing guidance that parser state must represent validated input, not convenient initialization values.
- !16920 (merged release-4.2): SOME/IP dynamic UAT-generated fields are explicitly invalidated/regenerated when UAT/profile state changes even when the dissector is not currently port-bound. Useful lifecycle evidence, but a backport and already consistent with notebook state/lifecycle guidance.
- !16910 (merged master): GNSS Galileo handoff now requires both even and odd pages to be nominal before using decoded data-word state; also resolves a Clang Analyzer finding. Corroborates validating all semantic prerequisites before downstream handoff.
- !16909 (merged master, Gerald Combs): Hungarian translation assets and developer-guide workflow updates. Useful maintenance documentation, but no broadly reusable coding/architecture rule.

## Notebook action

No convention file changed in this run. The durable observations reinforce existing guidance on stream reassembly/framing, validated parser state, downstream handoff preconditions, lifecycle invalidation, and weighting master changes above backports.

## Continuation

Rebuild the reviewed set from all tracking before the next run. Subject to that rebuild and the current corpus snapshot, the next descending candidate is !16908.
