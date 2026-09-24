# Automated MR review ledger: !11013–!11062

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Notebook head before this run: `f8cb57d3cf7262c33f0da27fdcd21cb9df56a2cd`

Selection was performed from the exact already-reviewed MR set reconstructed from the available per-run ledgers in `reviewed-mrs-automation/` plus `reviewed-mrs.md`. The previously reviewed !17571–!17620 batch remains counted. The prior run's !11062 mention was a frontier probe only, not a completed review. No numeric interval was treated as reviewed merely because a ledger covered neighboring numbers.

## Exact MRs reviewed in this run

1. !11062 — merged
2. !11061 — merged; deep review, 3GPP log timestamp parser forward progress
3. !11060 — closed/unmerged; down-weighted stable-branch proposal
4. !11059 — merged
5. !11058 — closed/unmerged; down-weighted, with Guy Harris discussion retained as contextual evidence
6. !11057 — merged
7. !11056 — merged
8. !11055 — merged
9. !11054 — merged; deep review, zero-length loop progress and TVBuff copying
10. !11053 — merged; deep review, all-set field-mask checker and registrations
11. !11052 — merged
12. !11051 — merged stable backport of !11048; lower independent weight
13. !11050 — merged
14. !11049 — merged
15. !11048 — merged; deep review, normalized conversation lookup/create identity
16. !11047 — merged
17. !11046 — merged
18. !11045 — merged
19. !11044 — merged; deep review, exception-boundary cleanup in WSLua
20. !11043 — merged
21. !11042 — merged
22. !11041 — merged
23. !11040 — merged
24. !11039 — merged
25. !11038 — merged; deep review, explicit SMB2 wire parsing instead of packed C-struct overlay
26. !11037 — merged
27. !11036 — merged
28. !11035 — merged
29. !11034 — merged
30. !11033 — merged
31. !11032 — merged
32. !11031 — merged
33. !11030 — merged
34. !11029 — merged
35. !11028 — merged
36. !11027 — merged
37. !11026 — merged
38. !11025 — merged
39. !11024 — merged
40. !11023 — merged
41. !11022 — merged
42. !11021 — merged
43. !11020 — merged
44. !11019 — merged
45. !11018 — merged
46. !11017 — merged
47. !11016 — merged
48. !11015 — merged
49. !11014 — merged
50. !11013 — merged

Disposition summary: **48 merged, 2 closed/unmerged (!11060 and !11058)**. Merged master changes were weighted most heavily; stable backports, closed proposals, and superseded/withdrawn directions were treated as corroboration or negative evidence rather than equal architectural precedent.

## Durable notebook updates promoted from this batch

- `parser-progress-conventions.md` — commit `9e303e0b572c0f3152bd0547a86ad029da8c07e2`.
  - !11061: retained/display precision and consumed input length are separate contracts; a bounded result must still consume the complete syntactic field so long inputs cannot produce a no-progress loop.
  - !11054: reject zero-length packet-derived loop elements before the next iteration; use TVBuff-native copying rather than extracting raw pointers for ordinary packet-byte copies.
- `conversation-key-normalization-conventions.md` — commit `4098dbdffb675214c1ec51d0dae249d899dae61d`.
  - !11048, strongly corroborated by stable !11051: create-on-miss must use the same reconstructed/normalized logical key as lookup. A generic helper that recreates identity from mutated `pinfo` can create a different conversation from the one just searched.
- `field-mask-semantics-conventions.md` — commit `7fef94ac4c6b1c004fcb147e5bf1d9f40dec4077`.
  - !11053: a standalone field representing the complete numeric value should normally use mask 0; full-width masks are suspicious except for the narrow whole-header `proto_tree_add_bitmask()` idiom later formalized by !11698.
- `wire-structure-decoding-conventions.md` — commit `90de24194978f7161ba26bb1b1778ce7c77a689e`.
  - !11038: decode packet structures from explicit offsets, widths, and endianness with TVBuff APIs instead of casting packet bytes to packed C structs or relying on compiler packing pragmas. Peter Stuge's and Pascal Quantin's review feedback was given substantial weight because the accepted implementation converged on that approach.

## Strong corroborating/contextual evidence not duplicated as new rules

- !11044 broadens the WSLua wrapper's caught recoverable exception set so the wrapper can report the child failure and restore its own Lua/packet state before cleanup. This reinforces the notebook's existing ownership/cleanup principle: a boundary that owns mutable local state must restore it on every recoverable exit, while intentionally fatal/computed exceptions still propagate.
- !11027 and !11046 avoid reading uninitialized WLAN addresses by explicitly representing absent addresses as `AT_NONE`; useful corroboration that optional packet metadata must have an explicit absence state before it participates in hashing/state lookup.
- !11015 reverts using EOF as a request-completion mechanism for a streaming RPC that can carry multiple sequential commands on one long-lived session. EOF is transport lifetime, not a generic application-message delimiter.
- !11016 reinforces that callers should use the actual storage contract of generated strings: when a `wmem_strbuf` token is guaranteed NUL-terminated, pass the API the representation it actually owns instead of maintaining a parallel packet-derived display length.
- !11050 and !11047 reinforce the previously recorded preference for named `register_dissector()` identities when a dissector should be discoverable by generic consumers.
- !11022 corrects checksum validation to use the protocol-defined MPLS-over-UDP scope, another example of deriving verification boundaries from the wire specification rather than nearby buffer extent.
- !11058 was closed/unmerged and is not implementation precedent. Guy Harris's discussion remains useful negative/contextual evidence: if the resolver returns a numeric-looking string as a host name, code cannot assume the lexical form proves that no name was resolved; stronger provenance is needed.
- !11060 was closed/unmerged after discussion of whether the proposed change belonged on a stable branch; it was down-weighted consistently with the notebook's fixes-not-enhancements stable-branch policy.

## Frontier

!11012 (`JSON 3GPP: fix a typo in hf name`) exists in the same corpus snapshot and is merged. It was inspected only to establish the descending frontier and **was not reviewed or counted in this run**. The corpus therefore has not run out.