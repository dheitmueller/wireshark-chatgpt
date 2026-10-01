# Convention synthesis — Wireshark MRs !2311–!2360

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Wiretap and capture formats

- Put format-recognition semantics in the open/probe path. Once a reader owns an accepted format, malformed or short records are read/bad-file errors, not "not my format." Evidence: Guy-authored !2349/!2350, with precursor !2335/!2342.
- Keep legacy file-type aliases beside the file-format module that owns the canonical type. Avoid a central compatibility map that must be edited separately. Evidence: Guy-authored !2359.
- For typed optional metadata, decode the payload only after identifying a known type and ignore unknown extension types when the format permits that. Evidence: Guy-authored !2345/!2346.

## Dissector registration and defaults

- Named `register_dissector()` registration is for direct lookup/fixed-handle consumers; table-only registration is valid when dispatch is keyed through a dissector table. Evidence: direct Guy Harris review in merged !2322.
- An unconfigured/default dissector must not accidentally become a catch-all. Audit zero masks, wildcard ranges, and fallback registrations for this failure mode. Evidence: merged !2356.

## Field and parser semantics

- Keep a presence indicator distinct from the actual value it guards when both are protocol-visible concepts. Evidence: merged !2360.
- Prefer established tree/bitmask/cursor APIs over a private table-driven mini-language when ordinary explicit dissection is clearer to project reviewers. Evidence: Anders Broman and Alexis La Goutte review in merged !2339.
- Use `proto_tree_add_item_ret_*`/display-string helpers when their return/display semantics match the parser's needs rather than manually duplicating fetch/format work. Evidence: Guy-authored !2346.

## Metadata inference

- Do not assume a capture-header field's historical name guarantees its semantic meaning. Cross-check it against packet-local evidence. Derive packet properties from the strongest available signals and explicitly mark unavailable details as unavailable. Evidence: Guy-authored !2311/!2323/!2332.

## Build and compiler review

- A diagnostic from a new compiler can expose a real C interface-contract bug; fix true declaration/definition mismatches regardless of compiler maturity. Verify actual bounds before weakening array declarations to silence the warning. Evidence: Guy Harris and Peter Wu review in merged !2324.
- For dependency-version/header conflicts, build a minimal reproducer and inspect actual compiler include-search ordering before choosing the workaround. Evidence: merged !2328.

## Dependency and test notes

- Before adding a new optional third-party dependency, check whether an existing supported dependency can provide the capability; account for support/packaging cost on every platform. Evidence: closed !2353, down-weighted but direct Pascal Quantin review.
- New WSLua dispatch APIs should have Lua regression tests. Also consider reentrant/self-dispatch cases explicitly for heuristic lists. Evidence: merged !2354 plus post-merge recursion report.

## Weighting notes

- !2353 and !2352 are closed/unmerged and serve only as lower-weight negative/history evidence.
- Stable backports corroborate the master design but are not independent architecture decisions.
