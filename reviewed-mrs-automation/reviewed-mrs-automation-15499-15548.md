# Reviewed Wireshark MRs — automation batch !15499–!15548

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `130eea3c8a60902c0fc98c9e1130e73bafc6f815`

Selection method: rebuilt the already-reviewed set from the available notebook review tracking, including `reviewed-mrs.md`, the aggregate automation tracking, and the per-run ledgers under `reviewed-mrs-automation/`. The historical !17571–!17620 batch is explicitly preserved and counted. Selection is by exact MR-number set subtraction against the corpus, not by assuming that a numeric interval is covered because some members appear in a ledger. No prior tracking entry covered any MR in this batch, so the fifty highest-numbered available unreviewed MRs are the contiguous set !15548 through !15499.

## Exact reviewed MR set

- !15548, !15547, !15546, !15545, !15544, !15543, !15542, !15541, !15540, !15539
- !15538, !15537, !15536, !15535, !15534, !15533, !15532, !15531, !15530, !15529
- !15528, !15527, !15526, !15525, !15524, !15523, !15522, !15521, !15520, !15519
- !15518, !15517, !15516, !15515, !15514, !15513, !15512, !15511, !15510, !15509
- !15508, !15507, !15506, !15505, !15504, !15503, !15502, !15501, !15500, !15499

Count: **50**.

## Review notes and weighting

- **!15540 — closed/unmerged; down-weighted.** WSLua warning-cleanup proposal. It was inspected but not treated as accepted project policy.
- **!15504 — deep, merged; high-authority corroboration.** Authored and merged by Guy Harris. Shared external state should have one authoritative declaration in a shared header rather than independent private `extern` declarations whose types can silently diverge. This strongly corroborates the existing linkage/interface rule in `build-conventions.md`, so no duplicate convention was added.
- **!15515 + !15520 — deep, merged; warning diagnosis and semantic repair.** !15515 initialized values after GCC/LTO reported possible use-before-initialization. Guy Harris explicitly asked whether the reports represented real use-before-set bugs or false positives; John Thacker subsequently identified !15520 as the better fix. !15520 makes `Dot11DecryptFtDerivePtk()` propagate failure instead of allowing callers to continue with invalid/uninitialized derived state. This reinforces the existing notebook rule that a new diagnostic should trigger semantic investigation rather than cosmetic warning suppression.
- **!15539 — deep, merged; promoted.** The accepted MySQL revert documents a conflict between documented/server handshake semantics and Connector/Python behavior. The durable rule is to retain documented protocol semantics as the default, isolate known implementation deviations rather than silently redefining the wire format, and be willing to revert a speculative broad interpretation while evidence remains ambiguous. Added to `protocol-implementation-compatibility-conventions.md`.
- **!15535 — deep, merged; promoted.** John Thacker replaces the assumption that the first captured TCP packet is the initial SYN with layered inference: SYN means destination server, SYN-ACK means source server, otherwise use the lower-numbered-port heuristic. This establishes the partial-capture rule that capture-start is not conversation-start and protocol evidence should precede heuristic fallback. Added to `partial-capture-inference-conventions.md`.
- **!15534 — deep, merged stable-branch fix; corroboration.** MATE ranges now carry their `tvbuff_t` data-source identity because offsets are meaningful only within the corresponding data source. When a field is in a different data source, ancestor/tree context must be consulted rather than comparing naked offsets. Strong evidence for preserving coordinate-space identity with offsets; not promoted as a duplicate rule in this run.
- **!15528 — deep, merged; corroboration.** Kafka array parsing checks that the declared count is plausible for remaining bytes and that each element decoder advances the offset. The fuzz-found failure was an effectively infinite loop caused by a huge count plus zero progress. This reinforces the existing parser progress/resource-bound rules.
- **!15516 — deep, merged; promoted.** John Thacker makes unsupported Kafka API versions decode using a deterministic supported-version fallback with expert information, specifically avoiding paths that dissect nothing and fail to advance. Added to `protocol-implementation-compatibility-conventions.md`: best-effort version substitution must be visible, bounded, and progress-preserving.
- **!15503 — deep, merged; promoted.** John Thacker, approved/merged by Anders Broman, removes use of DTLS `legacy_record_version` as an internal recognition gate once dispatch has already selected DTLS; RFC 9147 says that field must be ignored for all purposes. Added to `protocol-implementation-compatibility-conventions.md`: legacy/reserved fields that the current standard declares semantically ignored should not become rejection gates.
- **!15499 — deep, merged; promoted.** The ZigBee TLV recursion-limit path had returned the remaining tvbuff length even though it did not know the current nested value's actual length. That invented consumption boundary enabled caller backtracking and an infinite loop. The accepted fix returns the current offset and lets callers treat the remainder as unknown. Added to `dissector-consumption-boundary-conventions.md`.
- The remaining MRs were reviewed for accepted-change shape, discussion signal, and whether they added durable project-wide guidance. The batch contains a number of callback `int`→`bool` cleanups, automatic generated-data/translation refreshes, focused UI fixes, stable-branch backports, and narrow dissector corrections. They did not justify additional notebook rules beyond the conventions already present or promoted above.

## Notebook updates from this run

- `dissector-consumption-boundary-conventions.md`: added the rule that guard/failure paths must not invent a consumed boundary when the current item length is unknown (!15499).
- `protocol-implementation-compatibility-conventions.md`: new file covering documented semantics vs implementation deviations (!15539), semantically ignored legacy fields (!15503), and explicit/progress-preserving best-effort version fallback (!15516).
- `partial-capture-inference-conventions.md`: new file covering protocol-evidence-first role inference for midstream captures (!15535).

## Continuation

`mr_15498.json` exists at the same corpus commit, so the corpus is not exhausted and the next run can continue below this batch.