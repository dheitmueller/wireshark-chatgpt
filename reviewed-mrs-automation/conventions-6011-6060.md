# Durable conventions from !6011–!6060

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

- !6023/!6044/!6045/!6057: centralize multiplexed RTP/RTCP state in shared setup; preserve the intended front-door dissector; infer incomplete negotiation only when unambiguous; reject clear protocol nonmatches so fallback dispatch can run.
- !6036: fix ASN.1 generated field-name/type collisions in conformance input, regenerate, and run extra warning/registration checks.
- !6028: one filter-field identity should have one wire width/domain; use distinct fields for incompatible encodings and keep type, mask, and container width consistent.
- !6027: parse declared-length child lists inside a bounded subset TVBuff and verify each child fits before dispatch.
- !6013/!6014: malformed packet lengths are validation failures, not assertion invariants. !6015/!6016, !6038, and !6049 reinforce explicit legal-empty handling at helper boundaries.
- !6041: build downloads must treat HTTP failure as failure and use explicit fallback locations.
- !6042: Jaap Keuter recommends component-prefixed subjects, squashed history, shared constants, correct encoding metadata, and MRs that are small/single-topic but complete.
- !6039: process-owned capture options must cross the actual dumpcap process boundary; platform portability and UI scope are part of correctness.
