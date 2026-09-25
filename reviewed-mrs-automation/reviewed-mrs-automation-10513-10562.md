# Automated MR review ledger: !10513–!10562

Reviewed using GPT-5.6 Sol.

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Exactly 50 previously unreviewed MRs were reviewed. Candidate membership was checked individually against the available review tracking. The historical !17571–!17620 ledger was re-opened and still contains all 50 members. !10562 appeared previously only as a next-frontier probe, not as a completed review.

Status mix: **44 merged, 6 closed/unmerged**: !10559, !10542, !10541, !10531, !10525, !10522.

## Exact reviewed MR set

!10562 !10561 !10560 !10559 !10558 !10557 !10556 !10555 !10554 !10553
!10552 !10551 !10550 !10549 !10548 !10547 !10546 !10545 !10544 !10543
!10542 !10541 !10540 !10539 !10538 !10537 !10536 !10535 !10534 !10533
!10532 !10531 !10530 !10529 !10528 !10527 !10526 !10525 !10524 !10523
!10522 !10521 !10520 !10519 !10518 !10517 !10516 !10515 !10514 !10513

Count: **50 unique MRs**.

## Promoted findings

- !10536 / !10531: persistent dissector state must not rely on temporary mutation across calls that may throw.
- !10545: transformed payload streams can require a second framing/reassembly layer after TCP reassembly.
- !10523: values retained in file/conversation state need backing storage with a matching lifetime.
- !10513: filter identifiers need stable programmatic identity; dynamic field registration must track actual field availability.
- !10542 / !10525: reproduce against the actual target branch and validate the packet/reproducer against the specification before making a dissector more permissive.

Full notes: `review-findings-10513-10562.md`.

## Next frontier

!10512 exists at the same corpus commit, is merged, and was inspected only as a frontier probe. It was not reviewed or counted in this run.
