
# Wireshark MR automation ledger — !10462 through !10413

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This is the authoritative exact-set ledger for this run. Exactly 50 previously unreviewed MRs were reviewed. Selection was by individual MR membership in the accumulated tracking; numeric spans were not treated as implicitly complete. The historical !17571–!17620 batch remains preserved and counted.

| MR | Outcome |
|---|---|
| !10462 | merged |
| !10461 | merged |
| !10460 | merged |
| !10459 | merged |
| !10458 | closed / down-weighted |
| !10457 | merged |
| !10456 | merged |
| !10455 | merged |
| !10454 | merged |
| !10453 | merged |
| !10452 | merged |
| !10451 | merged |
| !10450 | merged |
| !10449 | merged |
| !10448 | merged |
| !10447 | merged |
| !10446 | merged |
| !10445 | merged |
| !10444 | merged |
| !10443 | merged |
| !10442 | merged |
| !10441 | merged |
| !10440 | merged |
| !10439 | merged |
| !10438 | merged |
| !10437 | merged |
| !10436 | merged |
| !10435 | merged |
| !10434 | merged |
| !10433 | merged |
| !10432 | merged |
| !10431 | merged |
| !10430 | merged |
| !10429 | merged |
| !10428 | closed / down-weighted |
| !10427 | merged |
| !10426 | merged |
| !10425 | merged |
| !10424 | merged |
| !10423 | merged |
| !10422 | merged |
| !10421 | merged |
| !10420 | closed draft / down-weighted |
| !10419 | merged |
| !10418 | merged |
| !10417 | merged |
| !10416 | merged |
| !10415 | merged |
| !10414 | merged |
| !10413 | merged |

## Exact reviewed MR set

!10462, !10461, !10460, !10459, !10458, !10457, !10456, !10455, !10454, !10453, !10452, !10451, !10450, !10449, !10448, !10447, !10446, !10445, !10444, !10443, !10442, !10441, !10440, !10439, !10438, !10437, !10436, !10435, !10434, !10433, !10432, !10431, !10430, !10429, !10428, !10427, !10426, !10425, !10424, !10423, !10422, !10421, !10420, !10419, !10418, !10417, !10416, !10415, !10414, !10413

Count: **50 unique MRs**. Maximum: **!10462**. Minimum: **!10413**.

Merged: **47**. Closed/unmerged: **3** (!10458, !10428, !10420).

## Promoted/corroborating findings

- !10441: persistent SSH keylog cache ownership, invalidation, and shutdown lifetime.
- !10460 / !10423: separate reusable PDU decoding from transport/representation framing.
- !10456: versioned conversation state for random-access redissection.
- !10445: initialize reporting before configuration/recent-file parsing can invoke it.
- !10453: named `register_dissector()` identities for reusable/discoverable dissectors.
- !10427: common charset/encoding implementation rather than protocol-private translation.
- !10454, !10438, !10439, !10452: generated-source, allocator, backport-scope, and checker-scope corroboration.

Full notes: `review-findings-10413-10462.md`.

## Next frontier

MR !10412 (`Gitlab templates: Be more forceful asking for a capture`) exists at the same corpus commit, is merged, and was inspected only as the next-frontier probe. It was not reviewed or counted.
