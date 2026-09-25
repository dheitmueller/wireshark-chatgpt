# Wireshark MR automation ledger — !10412 through !10363

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This is the authoritative exact-set ledger for this run. Exactly 50 previously unreviewed merge requests were reviewed. Selection was based on individual MR-number membership in the accumulated tracking, not inferred numeric range coverage. The historical !17571–!17620 batch remains preserved and counted.

| MR | Outcome |
|---|---|
| !10412 | merged |
| !10411 | merged |
| !10410 | merged |
| !10409 | merged |
| !10408 | merged |
| !10407 | merged |
| !10406 | merged |
| !10405 | merged |
| !10404 | merged |
| !10403 | merged |
| !10402 | merged |
| !10401 | merged |
| !10400 | merged |
| !10399 | merged |
| !10398 | closed / superseded / down-weighted |
| !10397 | merged |
| !10396 | merged |
| !10395 | merged |
| !10394 | merged |
| !10393 | merged |
| !10392 | merged |
| !10391 | merged |
| !10390 | merged |
| !10389 | merged |
| !10388 | merged |
| !10387 | merged |
| !10386 | merged |
| !10385 | open / down-weighted |
| !10384 | merged |
| !10383 | merged |
| !10382 | merged |
| !10381 | merged |
| !10380 | merged |
| !10379 | open / down-weighted |
| !10378 | merged |
| !10377 | merged |
| !10376 | merged |
| !10375 | merged |
| !10374 | merged |
| !10373 | merged |
| !10372 | merged |
| !10371 | merged |
| !10370 | merged |
| !10369 | merged |
| !10368 | merged |
| !10367 | merged |
| !10366 | merged |
| !10365 | merged |
| !10364 | merged |
| !10363 | merged |

## Exact reviewed MR set

!10412, !10411, !10410, !10409, !10408, !10407, !10406, !10405, !10404, !10403, !10402, !10401, !10400, !10399, !10398, !10397, !10396, !10395, !10394, !10393, !10392, !10391, !10390, !10389, !10388, !10387, !10386, !10385, !10384, !10383, !10382, !10381, !10380, !10379, !10378, !10377, !10376, !10375, !10374, !10373, !10372, !10371, !10370, !10369, !10368, !10367, !10366, !10365, !10364, !10363

Count: **50 unique MRs**. Maximum: **!10412**. Minimum: **!10363**.

Merged: **47**. Closed/unmerged: **1** (!10398). Open at the corpus snapshot: **2** (!10385, !10379).

## Promoted and corroborating findings

- !10412: Wireshark's issue templates explicitly make representative captures primary evidence for non-trivial bugs and useful for enhancements; screenshots are not a substitute.
- !10407, !10397, !10396: value-string fallback semantics and the project checker for detecting invalid `val_to_str` / `rval_to_str` helper usage.
- !10393/!10394 and !10395/!10408: Guy Harris-authored structured capture-open status and cause-specific diagnostics.
- !10389 with !10391/!10392: Guy Harris-authored separation of byte-stream framing wrappers from complete-PDU media-type entry points.
- !10376 with !10380/!10381: Guy Harris-authored adapter entry points where dissector-table `data` contracts differ.
- !10371: Guy Harris-authored extraction of the Internet `media_type` registry and context contract from HTTP into neutral ownership.
- !10405: merged new-dissector review required a representative pcap, fuzzing, checker cleanup, portable code, and a reviewable built-in source layout.
- !10374 and !10363: John Thacker-authored MPEG stream-type extension point and propagation of current PMT stream-type state into PES dispatch.
- !10403: static analysis exposed allocation-before-validation ownership leakage in SSH key material parsing; fixed before merge.
- !10385 and !10379 were not treated as accepted architecture because they remained open; !10398 was closed as superseded.

Full batch notes: `review-findings-10363-10412.md`.

## Next frontier

MR !10362 is the next descending candidate if it exists in the same corpus commit and has not been reviewed elsewhere. It was not counted in this run.
