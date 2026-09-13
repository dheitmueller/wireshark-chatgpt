# Wireshark MR review automation: !21291-!21340

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`
Review direction: newest to older
Exact MRs reviewed in this run: 50

Before selecting this batch, the available per-run ledgers under `reviewed-mrs-automation/` and the sparse tracking in `reviewed-mrs.md` were consulted and combined as the already-reviewed set. Selection was performed by MR-number set membership rather than by assuming ranges were complete. The immediately preceding exact batch covers !21341-!21390, and neither `reviewed-mrs.md` nor the per-run ledgers identified a previously reviewed exception among !21291-!21340. The historical !17571-!17620 batch remains preserved and counted. Thus the exact set below is the 50 highest-numbered MRs present at the corpus commit that had not already been reviewed.

Weighting policy: merged master changes and substantive maintainer feedback carry the most weight; release backports primarily corroborate their master changes; closed, draft, abandoned, or superseded work is down-weighted and is not treated as accepted implementation precedent. Highly authoritative maintainer evidence, especially Guy Harris's design and correctness guidance, is weighted accordingly.

## Exact reviewed set

- !21340
- !21339
- !21338
- !21337
- !21336
- !21335
- !21334
- !21333
- !21332
- !21331
- !21330
- !21329
- !21328
- !21327
- !21326
- !21325
- !21324
- !21323
- !21322
- !21321
- !21320
- !21319
- !21318
- !21317
- !21316
- !21315
- !21314
- !21313
- !21312
- !21311
- !21310
- !21309
- !21308
- !21307
- !21306
- !21305
- !21304
- !21303
- !21302
- !21301
- !21300
- !21299
- !21298
- !21297
- !21296
- !21295
- !21294
- !21293
- !21292
- !21291

## Review highlights and weighting notes

- **!21340 — deep / promoted, merged master, high confidence.** John Thacker changed the common compiler flags so UBSan builds do not use `-fno-strict-overflow`. Because that option implies `-fwrapv`, it defines signed overflow as wrapping and prevents the ordinary UBSan configuration from diagnosing it. Promoted to `sanitizer-configuration-conventions.md`: sanitizer build flags must preserve the language behavior the instrumentation is intended to detect.
- **!21333 — deep / promoted, merged master.** Wiretap now stores an explicit per-file start timestamp for formats such as BLF and LOG3GPP instead of conflating capture start with the first timestamped record. Promoted to `capture-start-time-conventions.md`: preserve authoritative file-level start state and derive dependent relative timing from it.
- **!21296 — deep / strong corroboration, merged master.** John Thacker's ClassicSTUN compatibility heuristic handles known nonconforming padding only when structural remaining-length evidence and semantic plausibility discriminate between padded and unpadded interpretations, and reports the anomaly with expert information. This reinforces existing conservative heuristic-parsing guidance without requiring a duplicate notebook rule.
- **!21312 — strong ownership corroboration, merged.** K12 creates its source-description hash with a value destroy function so replacing a duplicate key destroys the old owned value instead of leaking it. This reinforces the existing ownership/container guidance that replacement semantics must account for the displaced value.
- **!21315 — arithmetic-safety corroboration, merged stable backport.** Pcap length adjustment uses checked subtraction before removing AIX FDDI padding, preventing malformed small lengths from underflowing into huge allocation/copy sizes.
- **!21291 — strong buffer-sizing corroboration, merged stable backport authored/merged by Guy Harris.** LOG3GPP buffer reservations now include every serialized header component, including protocol parameters, and distinguish text from binary payload sizing. The previous omission happened to be masked by allocator spare capacity; that slack is not treated as a safety contract.
- **!21294 and !21292 — provenance/licensing corroboration, merged stable backports with Guy Harris involvement.** License checking distinguishes identifiers from license text instead of producing misleading duplicate classifications, while the c128 import records the actual license text's semantics rather than blindly repeating an upstream label. These reinforce existing source-provenance and contribution-licensing notes.
- **!21297 — compatibility/migration corroboration, merged master.** Built-in hfids move to zero initialization while registration temporarily tolerates third-party plugins that still use the historical `-1` sentinel, illustrating staged migration when external plugin code may depend on an older convention.
- **!21323 — draft/open / deliberately down-weighted.** The proposed HTTP/2 and HTTP/3 desegmentation fix remained incomplete; reviewer testing reproduced additional BoundsErrors and state pollution. It is useful negative evidence but not accepted implementation precedent.
- **!21322 — open/unmerged / review guidance only.** Guy Harris's discussion of renaming the macOS DMG highlights that artifact names are user/downstream compatibility surfaces, not merely developer-shell conveniences. Because the MR was not merged, this is not treated as accepted packaging precedent.
- **!21295 — closed/superseded / deliberately down-weighted.** The branch was accidentally force-updated with unrelated commits after the original small leak fix. Its final diff is not a coherent accepted implementation and is not used as precedent.

The remaining MRs in the exact set were reviewed for state, diff, discussion, maintainer input, and relevance but did not justify a new durable notebook rule beyond conventions already captured.
