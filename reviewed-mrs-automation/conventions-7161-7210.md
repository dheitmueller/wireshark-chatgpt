# Durable conventions from Wireshark MRs !7161–!7210

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

- !7209/!7202: keep source-model column identities stable; use proxy/view state for column visibility.
- !7201: prefer typed fvalue accessors over generic pointer retrieval and caller casts.
- !7180: keep Decode As reset/default, explicit Data, and no explicit binding as distinct semantics.
- !7173/!7182/!7183: check captured length before heuristic discriminator reads.
- !7163: wildcard UDP conversation handling must account for direction, protocol ownership, tuple reuse, recency, and lookup precedence.
- !7167: CLI option changes must update implementation, documentation, examples, and tests together.
- !7189 is historical precursor evidence; later merged !7228/!7235 remain the stronger AT_NUMERIC precedent.
