# Conventions extracted from !7561-!7610

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

- !7601: Use the tvbuff that actually defines the semantic payload. A parent tvbuff plus a former offset is not equivalent to an established payload child after reassembly.
- !7595: Read-only UI/filter paths should retrieve existing conversation state, not create conversations or assign stream numbers.
- !7587: When a protocol knows client/server direction, carry that semantic direction explicitly instead of inferring it from first-seen endpoints.
- !7586: Conversation/endpoint UI capabilities should derive from registered table fields and actual model semantics, not protocol-name guesses.
- !7584: Shared event-loop and capture-pipe plumbing belongs in common capture code rather than duplicated UI frontends.
- !7583: Do not use a typedef'd signed integer such as `gboolean` as a one-bit boolean bitfield. Use standard `bool`.
- !7581: Prefer automatic dissector-table preferences over duplicated range state and preference callbacks.
- !7580: Protocol filter/module names and dissector short names are distinct identifier domains; preference migration must use the identifier expected by each lookup API.
- !7567: Once valid tap metadata exists, make publication exception-safe and exactly once.
- !7564: Capture-producer normalization that departs from nominal wire lengths should be explicit policy, with user-visible diagnostic guidance.
- !7571/!7573/!7574: Generated documentation should avoid source-mtime-dependent output when reproducibility matters.

Closed !7610 provides useful review reasoning about TCP state-update ordering, but its proposed implementation is not accepted precedent.
