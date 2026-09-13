# Wireshark MR review automation: !21341-!21390

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`
Review direction: newest to older
Exact MRs reviewed in this run: 50

Before selecting this batch, the available per-run ledgers under `reviewed-mrs-automation/` and the sparse tracking in `reviewed-mrs.md` were consulted and combined as the already-reviewed set. The immediately preceding contiguous batch covers !21391-!21440. No sparse previously-reviewed exception was found among !21341-!21390. The historical !17571-!17620 batch remains preserved and counted. Thus the exact set below is the 50 highest-numbered MRs present at the corpus commit that had not already been reviewed.

Weighting policy: merged master changes and substantive maintainer feedback carry the most weight; release backports primarily corroborate their master changes; closed/superseded work is down-weighted and is not treated as accepted implementation precedent. Later corrective evidence is also considered: in particular, !21384 was merged but was later explicitly identified as having introduced a bug fixed by !22292, so its implementation is not treated as durable precedent.

## Exact reviewed set

- !21390
- !21389
- !21388
- !21387
- !21386
- !21385
- !21384
- !21383
- !21382
- !21381
- !21380
- !21379
- !21378
- !21377
- !21376
- !21375
- !21374
- !21373
- !21372
- !21371
- !21370
- !21369
- !21368
- !21367
- !21366
- !21365
- !21364
- !21363
- !21362
- !21361
- !21360
- !21359
- !21358
- !21357
- !21356
- !21355
- !21354
- !21353
- !21352
- !21351
- !21350
- !21349
- !21348
- !21347
- !21346
- !21345
- !21344
- !21343
- !21342
- !21341

## Review highlights and weighting notes

- **!21367 — deep / promoted, merged release-4.6.** Guy Harris identified that the proposed BLF backport depended on `try_val_to_str()` at a layer where release-4.6 did not expose it. The successful path was to bring in the intended `value_string`-to-`wsutil` prerequisite via !21368 rather than inventing an improper Wiretap-to-epan dependency or local duplicate. Promoted to `stable-branch-submission-conventions.md` as a target-branch dependency/API-surface rule.
- **!21356 — deep / strong corroboration, merged master.** John Thacker fixed BER absolute end-offset overflow and explicitly stated that the superior long-term design is to eliminate the end-offset variable in favor of a subset TVB, so bounds failure occurs at actual access. Added as high-authority corroboration to the existing bounded-sub-TVB rule in `parser-boundary-conventions.md`.
- **!21375 — deep / promoted, merged master.** `ptvcursor` item helpers now funnel offset movement through `ptvcursor_advance()`, which performs checked addition and consistent bounds failure. Promoted as a cursor/API invariant rule in `parser-boundary-conventions.md`.
- **!21384 — merged but negatively weighted by later evidence.** The BER bit-string checked-length change was later called out by Stig Bjørlykke as introducing a bug fixed by !22292. It remains part of the reviewed audit set but is not used as accepted implementation precedent.
- **!21382 — closed/superseded / down-weighted.** Its implementation is not accepted precedent. Alexis La Goutte's request for a representative capture remains useful corroboration of the established capture-driven review convention.
- **!21358 — strong corroboration, merged first contribution.** The contributor supplied captures specifically exercising AMS traffic split across multiple TCP packets, reinforcing that representative captures should exercise the stateful/reassembly condition being changed.
- **!21359, !21389 — strong parser-corollary evidence.** Both demonstrate that helper side effects and cursor/remaining-length invariants must be understood precisely: X11 had double advancement because a field macro already moved the offset, while SIP needed remaining-line length adjusted when the cursor moved.
- **!21346, !21354 — strong semantic-validation corroboration.** SMPP rejects malformed date components rather than relying on libc normalization, and H.264 validates the specification's delta-scale range before arithmetic that could otherwise invoke undefined behavior.
- **!21342, !21344, !21345, !21364, !21385, !21390 — arithmetic-safety corroboration.** These merged fixes reinforce the existing notebook guidance to choose the correct semantic arithmetic domain, avoid signed undefined behavior, and use checked operations where packet-derived arithmetic can exceed implementation limits.
- **!21363 — public-header corroboration.** Adding `jtckdint.h` transitively to a public umbrella header exposed packaging requirements when Debian/Ubuntu builds lacked the installed dependency; this reinforces testing public headers as real consumer-facing interfaces with complete install-time dependencies.
- **!21361 — simplification corroboration.** H.264/H.265 Exp-Golomb handling removes a duplicated zero-value special path and lets the general path handle it, reducing opportunities for edge-case behavior to diverge from normal parsing.
- **!21366 — static-analysis review corroboration.** Maintainer discussion did not simply delete a dead-store line mechanically; it checked whether the lookup had historical or side-effect meaning. This reinforces treating analyzer warnings as prompts for semantic review, not blind rewrites.

The remaining MRs in the exact set were reviewed for state, diff, discussion, and relevance but did not justify a new durable notebook rule beyond conventions already captured.