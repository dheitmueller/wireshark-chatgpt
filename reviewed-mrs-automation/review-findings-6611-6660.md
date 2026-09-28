# Review findings: !6611-!6660

Corpus: `ddcaa22b51c68f594e425a23388c3a2086813054`. Reviewed 50 MRs: 45 merged, five closed.

Strongest findings:
- !6650 (John Thacker; Jaap Keuter review): Protocol Hierarchy must distinguish frames containing a protocol from protocol-PDU count.
- !6631 (Joao Valverde): context-typed literals should be tried before ambiguous tokens fall back to field resolution.
- !6628: normalize display-filter literal syntax once in the scanner; avoid retry paths that leak error state.
- !6621 + !6623 (John Thacker): an incomplete PDU caused by disabled/unavailable reassembly is a fragment/reassembly note, not malformed protocol data.
- !6629 with !6644/!6645: parser cursor follows encoded field width, not semantic prefix significance.
- !6626 with !6638: nested length excludes its own two-byte header; decode payload length and advance header + payload.
- !6642 (Gerald Combs): prefer squash-merging an MR to one commit unless distinct commits are justified.
- !6646: keep packet and log conversation-filter registries separate by semantic domain.
- Closed !6619 is superseded by merged !6694's generalized interface-type filtering; other closed MRs were not treated as implementation precedent.
