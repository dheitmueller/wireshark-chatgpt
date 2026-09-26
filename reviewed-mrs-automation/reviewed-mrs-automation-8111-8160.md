# Reviewed MR automation ledger: !8111-!8160

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook base: `342a03a17fbc23b2254480bf7527194e4b70e581`
- Review direction: descending from the highest previously unreviewed MR
- Reviewed in this run: **50**
- Merge status: **48 merged, 2 closed/unmerged (!8150 and !8147)**
- Historical batch !17571-!17620 remains preserved and counted as **50 unique reviewed MRs**.

## Exact reviewed set

!8160, !8159, !8158, !8157, !8156, !8155, !8154, !8153, !8152, !8151, !8150, !8149, !8148, !8147, !8146, !8145, !8144, !8143, !8142, !8141, !8140, !8139, !8138, !8137, !8136, !8135, !8134, !8133, !8132, !8131, !8130, !8129, !8128, !8127, !8126, !8125, !8124, !8123, !8122, !8121, !8120, !8119, !8118, !8117, !8116, !8115, !8114, !8113, !8112, !8111

## Selection validation

The corpus tip was verified as `ddcaa22b51c68f594e425a23388c3a2086813054`. The authoritative preceding notebook branch `automation/mr-review-8161-8210-complete` was consulted, including its exact !8161-!8210 ledger, together with `reviewed-mrs.md` and the historical !17571-!17620 exact ledger. Candidate references !8111 through !8160 were also searched individually against the repository's searchable review-tracking files; none appeared as an already-reviewed MR. The preceding exact ledger names !8160 only as the next frontier, not as reviewed.

The corpus contains every MR in !8111-!8160, so the fifty highest-numbered previously unreviewed MRs are exactly this contiguous interval. This conclusion follows from the explicit reviewed set plus corpus presence checks, not from assuming a numeric range was already covered.

The historical `reviewed-mrs-automation/reviewed-mrs-automation-17571-17620.md` ledger was re-opened and still records exactly 50 MRs from !17571 through !17620.

## Weighting note

Merged master changes and substantive maintainer guidance were weighted most heavily. Stable-branch backports were treated as corroboration. The two closed MRs were retained in the exact ledger but down-weighted: !8150 is an abandoned alternative around TCP tap delivery for issue #18138, while the accepted merged implementation is !7567; !8147's logging-severity proposal was closed and therefore is useful only as negative/design-discussion evidence.

## Next frontier

!8110 exists in the corpus, is merged on `release-4.0`, and is outside this run. It should be considered only as the next candidate frontier after this ledger.
