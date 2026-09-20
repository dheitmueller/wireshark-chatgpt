# Automated Wireshark MR review: !16148–!16099

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit: `130eea3c8a60902c0fc98c9e1130e73bafc6f815`
- Reviewed count: 50
- Selection method: consulted `reviewed-mrs.md` and the available per-run ledgers under `reviewed-mrs-automation/`, preserving and counting the historical !17571–!17620 batch. The immediately preceding exact-set ledger records !16198–!16149 and, using the corrected recursive-tree/set-subtraction procedure, explicitly identifies `mr_16148.json` as the next corpus entry. The corpus commit is unchanged. This run reviewed the next fifty individual corpus MRs, !16148 through !16099; no numeric interval was treated as reviewed merely because neighboring entries appeared in a ledger.
- Outcome weighting: all fifty MRs in this batch are merged. Maintainer-authored/reviewed changes were weighted according to their authority, particularly Guy Harris's Wiretap work and John Thacker's API/generator work.

## Exact reviewed MRs

- !16148
- !16147
- !16146
- !16145
- !16144
- !16143
- !16142
- !16141
- !16140
- !16139
- !16138
- !16137
- !16136
- !16135
- !16134
- !16133
- !16132
- !16131
- !16130
- !16129
- !16128
- !16127
- !16126
- !16125
- !16124
- !16123
- !16122
- !16121
- !16120
- !16119
- !16118
- !16117
- !16116
- !16115
- !16114
- !16113
- !16112
- !16111
- !16110
- !16109
- !16108
- !16107
- !16106
- !16105
- !16104
- !16103
- !16102
- !16101
- !16100
- !16099

## Durable findings promoted to the notebook

### Normalize a unifying display-filter field to one semantic value domain

Merged master !16118 adds one MAC-LTE LCID field intended to filter the same logical channel across directions. Pascal Quantin identified that the first implementation did not handle extended LCIDs consistently; the review distinguished raw extended-LCID codepoints from logical indexes, and Martin Mathieson revised the field so the unified filter exposes the semantic index across forms. Promoted to `protocol-field-semantic-conventions.md`.

### Reset dependent analysis state at protocol lifecycle boundaries

Merged master !16110 resets RLC and PDCP bearer state at LTE RRC events that invalidate prior sequence/history state. Pascal Quantin's review expanded the audit to additional event variants and NB-IoT while also identifying event-specific PDCP reset differences; Martin Mathieson deliberately left complex unsupported cases as TODOs instead of applying a blanket reset. Promoted to `state-refresh-conventions.md`.

### Keep each format recognizer format-specific; let orchestration own probe ordering

Merged master !16121, authored and merged by Guy Harris, splits Wiretap compression recognition into per-format routines and groups each format's recognition, documentation references, and decompression support together. The outer recognition path retains ordered probing and tri-state accept/reject/error handling. Promoted to `probe-control-flow-conventions.md`.

### Separate dissector machine identifiers from human-readable descriptions

Merged master !16140, authored by John Thacker and merged by Anders Broman, introduces an explicit name-versus-description distinction for unregistered/custom-table dissector handles. Related merged !16135 validates NULL/empty registered names at registration, !16123 reuses a common naming validator for preference aliases with the same grammar, and !16122 removes a duplicate uppercase/lowercase registration of the same dissector. Promoted to new `dissector-naming-conventions.md`.

### Reuse the canonical extended sequence representation

Merged master !16146, authored by John Thacker and merged by Anders Broman, removes RTP tap-local wrap-cycle tracking and uses the RTP dissector's already-derived extended sequence number. The old independent cycle logic could count extra wraps when out-of-order packets crossed zero again. Promoted to new `sequence-number-conventions.md`, with previously reviewed !16261 retained as corroborating wraparound-testing evidence.

## Other useful evidence

- !16148 independently corroborates the prior !16155/!16156 lesson that grammar sentinels such as `*` and `]` must be recognized before strict numeric conversion rather than relying on `atoi()`-style permissive fallback.
- !16147, authored and merged by Guy Harris, corrects BLF documentation so the model matches observed files: top-level objects may be log containers, but they need not all be containers. This reinforces the broader rule that parser comments and data models should describe observed format structure rather than stale assumptions.
- !16141 and its !16142 stable backport corroborate offset/return-contract guidance: loop offsets must use a type wide enough not to wrap, and a helper expected to return bytes consumed must not instead return an absolute offset.
- !16143 demonstrates disciplined static-analysis cleanup: the contributor limited clang-analyzer NULL-dereference fixes to cases supported by analogous existing NULL checks rather than blindly accepting every analyzer report.
- !16132, !16130, !16127, !16112, !16111, !16106, and !16105 reinforce the generated-source workflow: update the generator/authoritative inputs and regenerate checked-in dissectors/tests so generated output remains reproducible and synchronized.
- !16119 shows that moving a hand-written dissector to PIDL generation can preserve externally visible long name, short name, and description through generator/conformance inputs rather than accepting user-facing naming churn as a side effect of generation.
- !16117 includes concrete capture files for the newly supported IEEE 1905/EasyMesh values, reinforcing capture-backed dissector validation.
- !16128 prefers Qt's established Base64 decoder over ad-hoc handling because the library routine handles imperfect Base64 more robustly.
- !16104 through !16099 synchronize Wireshark's PIDL copy with upstream Samba. The series includes fixes for nondeterministic generated text, Python exception propagation, expression/operator precedence, big-endian UTF-16 handling, and typed flag domains; these are useful accepted upstream evidence but did not justify duplicating existing Wireshark-local generator/type-safety rules.

## Notebook commits

- `7ddfaa10297f38596027518c938fc3da9ccfdd1f` — normalize unified filter-field semantics from !16118
- `44cfb253034f1f501c69b0c501f884e72f076635` — protocol lifecycle state-reset convention from !16110
- `33536873c5b9d9744ab15477902708c8ca54155c` — format-specific recognition convention from !16121
- `14a0b1598a1652e30eb065fc55efd9c75c24b34a` — dissector naming conventions from !16140/!16135/!16123/!16122
- `d74e1eb7d3fadb35d447ecd3f2890e55918a3377` — canonical extended sequence-number convention from !16146

The corpus continues below this batch; `mr_16098.json` exists at the same corpus commit.
