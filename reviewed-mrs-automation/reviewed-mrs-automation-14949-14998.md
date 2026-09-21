# Wireshark MR review automation: !14949–!14998

Corpus repository commit: `1c7ad509887ee25079a7865cc62ba18cba06f49a`

Notebook baseline before this run: `642327bc49a35e0cd20b54e6e9f702fec342a622`

Selection method: rebuilt the already-reviewed set from all available review tracking in the notebook, including `reviewed-mrs.md`, the aggregate automation tracking, and the per-run files under `reviewed-mrs-automation/`. The historical !17571–!17620 batch was explicitly preserved and counted. Selection was by exact set subtraction against MR files present in the corpus, not by assuming that filename ranges implied complete review coverage.

The 50 highest-numbered corpus MRs not already in that reviewed set were !14998 through !14949 inclusive. This run reviewed exactly those 50. State weighting: 49 were merged; !14975 was closed/unmerged and was down-weighted as abandoned/superseded evidence.

## Exact reviewed MR set

!14998, !14997, !14996, !14995, !14994, !14993, !14992, !14991, !14990, !14989, !14988, !14987, !14986, !14985, !14984, !14983, !14982, !14981, !14980, !14979, !14978, !14977, !14976, !14975, !14974, !14973, !14972, !14971, !14970, !14969, !14968, !14967, !14966, !14965, !14964, !14963, !14962, !14961, !14960, !14959, !14958, !14957, !14956, !14955, !14954, !14953, !14952, !14951, !14950, !14949.

## Durable notebook findings promoted

- **!14992 (merged master; authored and merged by John Thacker): dependency-version guards must track the actual first affected upstream release, including maintenance-line backports.** A Qt behavior/regression relevant to SyntaxComboBox backgrounds had been backported to Qt 5.15.3, so a workaround that only covered later Qt versions missed a supported affected version. Stable !14993 and !14994 corroborate the correction. Added `dependency-version-compatibility-conventions.md` in notebook commit `754fb516b7a54e5b8328f34530693bccb830d861`.
- **!14990 (merged master; approved/merged by Anders Broman): pass explicit semantic context rather than reaching for ambient packet scope, and choose the parameter type according to the helper's responsibility.** Packet-oriented helpers should accept `packet_info *` and use `pinfo->pool`; narrowly allocation-oriented helpers should accept `wmem_allocator_t *`. Added `semantic-context-parameter-conventions.md` in notebook commit `c6bfbb0583844eb07d555ade24e0d222c602fb5d`.
- **!14985 (merged master; authored and merged by John Thacker): a validity flag guards the read itself, not merely the interpretation of a value.** `short_preamble` can be uninitialized when `has_short_preamble` is false, so even speculative access is invalid/undefined; the accepted path avoids reading it and explicitly assigns a fallback before later use. Added `optional-value-validity-conventions.md` in notebook commit `f0a00104bfb38c8f39cd8af5ce2c4b995c90eb02`.
- **!14970 (merged master; authored and merged by John Thacker): choose numeric representation per statistical operation.** I/O Graph totals can use `double` to avoid fixed-width integer accumulation overflow when the result is ultimately graphed as floating point and exact >2^53 integer precision is not part of the contract; minima/maxima retain exact 64-bit integers so ordering and the selected extrema frame remain correct. Added `statistics-numeric-representation-conventions.md` in notebook commit `0d11d1c5364317dca20117e7e064737fc9652e40`.

## Strong corroborating evidence retained

- **!14973 (merged master)** independently reinforces the existing structured-API evolution rule: sharkd kept the legacy positional/string-array `columns` field and added an extensible structured `column_info` field because the old shape could not gain metadata compatibly; unit tests cover the new result. This matches the already-recorded !15405 pattern, so no duplicate notebook rule was added.
- **!14974 + !14978 (merged master)** reinforce generated-code source-of-truth discipline. The generator change removed a non-portable Unicode arrow and John Thacker explicitly required regeneration of the ASN.1 dissectors; the follow-up regenerated outputs rather than hand-editing generated files.
- **!14984 (merged master, John Thacker)** reinforces arithmetic-safety guidance by keeping interval/timestamp arithmetic in sufficiently wide integer/time representations until conversion is actually required.
- **!14971 (merged master)** reinforces separation of canonical machine-readable data from display formatting: absolute-time I/O Graph CSV/clipboard export uses numeric epoch seconds rather than formatted GUI timestamps that had broken data export semantics.
- **!14995 (merged master)** reinforces protocol-semantic termination handling: LLDP TTL zero denotes a Shutdown PDU, changes interpretation of the whole message, and extra TLVs after it are diagnosed; the submission included a sample capture.
- **!14988 (merged master)** reinforces explicit invariant assertions for programmer-error states rather than allowing an impossible missing heuristic-dissector list to fail indirectly.
- **!14950 and !14949 (merged master, with stable backports in this batch)** reinforce nullability contracts: static-analysis-discovered nullable dissector state/child buffers must be checked before dereference. Stable cherry-picks were treated as supporting evidence rather than weighted as independent design decisions.

## Lower-signal and maintenance material

The remaining merged MRs were also inspected. They mainly consisted of release preparation, stable cherry-picks, protocol/field updates, Qt I/O Graph behavior fixes, build/dependency maintenance, documentation corrections, and straightforward initialization/null-check cleanups. These were retained in this exact ledger but did not justify duplicating broader conventions already present in the notebook.

!14975 was a closed draft release-3.6 cherry-pick with conflicts/empty effective changes and was explicitly down-weighted; it was not used to establish a positive convention.

## Corpus frontier

`mr_14948.json` is present and is a valid merged MR at the same corpus commit, so the corpus is not exhausted. The next backward review can continue from !14948 unless a newer unreviewed MR appears in the corpus before that run.
