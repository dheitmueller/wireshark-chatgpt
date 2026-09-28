# Durable conventions from 5761-5810

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

- MR 5809 with MR 5772: registered display-filter names are user-facing compatibility surface; avoid casual renames and separate broad naming cleanup from unrelated feature work.
- MR 5801: presentation dissection must remain redissection-safe while persistent learned state stays first-pass-only.
- MR 5783 with release backports 5804 and 5805: validate capacity before dereference; finish the master fix first, then use separate stable-branch backport merge requests.
- MR 5766: use packet-scoped protocol data for packet-owned state and explicitly reset scratch values at the semantic boundary expected to refill them.
- MRs 5764, 5765, 5763, and 5796: generated dissector corrections belong in authoritative conformance or template inputs and regenerated artifacts, not only derivative C.
- MR 5810: preserve stable documentation anchors where links may already depend on them.
- MRs 5800 and 5785: optional-feature compilation paths must remain coherent at both registration and dependency-use boundaries.

Closed MRs 5808 and 5765 were down-weighted and were not treated as accepted implementation precedent.
