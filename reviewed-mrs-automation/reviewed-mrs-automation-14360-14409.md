# Automated MR review ledger: !14360-!14409

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `1c7ad509887ee25079a7865cc62ba18cba06f49a`
Notebook starting commit: `8807f28c0c855ad4ef2f0edf1c0cdf536acb964d`
Review direction: descending from the highest-numbered previously unreviewed MR.

## Selection and deduplication

Before selecting this batch, the reviewed set was reconciled against `reviewed-mrs.md`, the aggregate automation tracking file, and the complete available per-run ledger inventory under `reviewed-mrs-automation/`, including the immediately preceding exact ledger `reviewed-mrs-automation-14410-14459.md`. The historical !17571-!17620 batch remains explicitly included in the reviewed set. Later sparse/high-numbered runs, including the !265xx material, were also retained by exact MR-number membership rather than inferred numeric ranges.

At selection time the notebook was exactly at the commit produced by the preceding run and the corpus was still at `1c7ad509887ee25079a7865cc62ba18cba06f49a`; therefore the preceding reconciliation remained current. Exact membership subtraction identifies the fifty highest-numbered available unreviewed MRs as !14409 through !14360. This run reviews exactly those fifty and no others.

Outcome: **50 merged MRs**. There are no closed/unmerged or still-open MRs in this batch, so no batch member required state-based down-weighting.

## Exact MRs reviewed

| MR | State | Review depth |
|---|---|---|
| !14409 | merged | Scanned |
| !14408 | merged | Scanned |
| !14407 | merged | Discussion/scanned |
| !14406 | merged | Scanned |
| !14405 | merged | Scanned |
| !14404 | merged | Scanned |
| !14403 | merged | Scanned |
| !14402 | merged | Corroborating |
| !14401 | merged | Corroborating |
| !14400 | merged | Scanned |
| !14399 | merged | Scanned |
| !14398 | merged | Scanned |
| !14397 | merged | Scanned |
| !14396 | merged | Discussion-focused |
| !14395 | merged | Scanned |
| !14394 | merged | Deep / promoted |
| !14393 | merged | Scanned |
| !14392 | merged | Scanned |
| !14391 | merged | Scanned |
| !14390 | merged | Corroborating |
| !14389 | merged | Scanned |
| !14388 | merged | Scanned |
| !14387 | merged | Scanned |
| !14386 | merged | Scanned |
| !14385 | merged | Scanned |
| !14384 | merged | Corroborating |
| !14383 | merged | Corroborating |
| !14382 | merged | Scanned |
| !14381 | merged | Scanned |
| !14380 | merged | Discussion/scanned |
| !14379 | merged | Scanned |
| !14378 | merged | Discussion/corroborating |
| !14377 | merged | Scanned |
| !14376 | merged | Scanned |
| !14375 | merged | Scanned |
| !14374 | merged | Corroborating |
| !14373 | merged | Corroborating stable counterpart |
| !14372 | merged | Deep / promoted |
| !14371 | merged | Scanned |
| !14370 | merged | Corroborating stable counterpart |
| !14369 | merged | Corroborating |
| !14368 | merged | Deep stable counterpart |
| !14367 | merged | Deep / promoted |
| !14366 | merged | Scanned |
| !14365 | merged | Corroborating stable counterpart |
| !14364 | merged | Deep/corroborating |
| !14363 | merged | Deep stable counterpart |
| !14362 | merged | Deep / promoted |
| !14361 | merged | Deep / promoted |
| !14360 | merged | Corroborating stable counterpart |

## Durable notebook changes promoted from this batch

- `capture-metadata-presence-conventions.md`: validate truncated pseudo-headers per field, and describe capture-record layouts with explicit offsets/lengths rather than relying on native C structure layout. Primary evidence: merged master !14367 and release-4.2 backport !14368, authored and merged by Guy Harris. Commit: `48dc7489c156bac3067319d2292eb69d8a669051`.
- `wire-encoding-conventions.md`: when byte order is a runtime property, every access to a field must use the same encoding decision; additionally, byte-swap compatibility preferences must remain scoped to the exact historical wire-format quirk they represent. Primary evidence: merged !14372/!14373 and !14362/!14363, with the master changes authored and merged by Guy Harris. Commit: `b66529c2183839ae1c6097bba6d7595dff7a35e2`.
- `wiretap-writer-conventions.md`: a reader that synthesizes a packet should use the native `WTAP_ENCAP_*` when it fully represents that packet instead of adding an unnecessary Upper-PDU wrapper and dissector-name tags; helper names should reflect generation versus file I/O. Primary evidence: merged !14394, authored and merged by Guy Harris. Commit: `cfc5ab15863a7ac3f205b2c9edcc9be6afcf7fb4`.
- `protocol-nesting-and-token-boundary-conventions.md`: splitting a language expression into outer components must respect real operator token boundaries and parenthesis/grouping depth rather than matching operator substrings blindly. Primary evidence: merged !14361, authored and merged by John Thacker. Commit: `8bf28b526dfd3d4df711265c1cc2199a43efdeeb`.

## Strong corroborating and review evidence retained without duplicate notebook rules

- !14402 frees the vector returned by `g_strsplit()` after stats-tree path processing, corroborating existing explicit ownership/lifetime guidance.
- !14396 and !14378 both contain maintainer requests for concrete capture samples before/alongside protocol-dissection changes; both contributors supplied captures, reinforcing existing sample-backed validation/submission guidance rather than creating a duplicate rule.
- !14374 hardens malformed-length handling so zero/progress-breaking lengths cannot trap parsing, reinforcing existing parser-progress and minimum-length conventions.
- !14364/!14365 and !14369/!14370 are part of the same SocketCAN normalization sequence as the promoted encoding/pseudo-header rules and reinforce that byte order belongs to the actual capture encapsulation contract.
- !14390 uses the semantically correct host-to-network conversion spelling even where the inverse operation would produce identical machine code, useful corroboration that API names should communicate direction/intent.
- !14360 fixes reuse of a `proto_item *` for a nested child before the original item was finished. It is retained as corroborating tree-item identity/lifetime evidence; because its master counterpart lies below this run's frontier, no new notebook rule is promoted from the stable backport alone.

## Weighting notes

This batch contains an unusually strong cluster of Guy Harris-authored-and-merged SocketCAN/wiretap fixes. Those MRs were given the highest evidentiary weight. John Thacker's authored-and-merged !14361 was likewise treated as strong parser/UI-expression precedent. Routine release/version updates, protocol-local field additions, generated-data maintenance, and changes without a reusable cross-cutting lesson were reviewed but not promoted into notebook rules.

## Frontier after this run

`mr_14359.json` exists in the same corpus commit and is merged. Therefore the corpus is not exhausted. If no newer previously unreviewed MR is added before the next run, !14359 is the next descending candidate.
