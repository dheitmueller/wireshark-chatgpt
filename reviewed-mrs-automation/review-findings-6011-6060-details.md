# Review findings details: !6011–!6060

Corpus: `ddcaa22b51c68f594e425a23388c3a2086813054`

Strongest evidence: John Thacker's !6057 and !6023/!6044/!6045 on multiplexed dispatch/state; !6036 on fixing ASN.1 field collisions in conformance input; !6028 on keeping one registered field identity to one width/domain; !6027 on bounded subset parsing for declared-length child lists; Gerald Combs's !6013/!6014 on validation instead of assertions for malformed wire lengths; !6015/!6016, !6038 and !6049 on explicit legal-empty handling; Gerald's review in !6041 on HTTP-status-aware dependency downloads; Jaap Keuter's post-merge !6042 guidance on focused, complete, squashed submissions; and !6039 on propagating capture configuration across the dumpcap process boundary.

Corroboration: !6056 and !6047/!6048 for typed proto-tree API contracts, !6055 for unaligned storage, !6051 for nounset-safe shell variables, and !6034 for nested TLS subdissector dispatch. Closed !6054 was superseded and is not implementation precedent.
