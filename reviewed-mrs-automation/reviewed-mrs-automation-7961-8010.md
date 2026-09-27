# Reviewed MR automation ledger: !7961-!8010

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`
Notebook base: `24f5baab9b13f836756053c74430a5bc7bbd19c3`
Model gate: GPT-5.6 Sol
Reviewed in this run: **50**
Merged: **49**
Closed/unmerged: **!7999**

## Selection and de-duplication

Before selection, review tracking was reconciled using `reviewed-mrs.md`, the immediately preceding exact !8011-!8060 ledger, the historical !17571-!17620 ledger, and candidate-by-candidate searches against the available `reviewed-mrs*` tracking on the repository default branch. No candidate from !7961 through !8010 was found as previously reviewed. The preceding ledger mentions !8010 only as its documented frontier probe and explicitly does not count it as reviewed.

The historical !17571-!17620 ledger was re-read and validated as exactly 50 unique MR numbers with no omissions. That batch remains preserved and counted.

## Exact reviewed set

!8010, !8009, !8008, !8007, !8006, !8005, !8004, !8003, !8002, !8001, !8000, !7999, !7998, !7997, !7996, !7995, !7994, !7993, !7992, !7991, !7990, !7989, !7988, !7987, !7986, !7985, !7984, !7983, !7982, !7981, !7980, !7979, !7978, !7977, !7976, !7975, !7974, !7973, !7972, !7971, !7970, !7969, !7968, !7967, !7966, !7965, !7964, !7963, !7962, !7961

## Outcome weighting

Merged master work and substantive maintainer review were weighted most heavily. Stable-branch backports were used mainly as corroboration. Guy Harris's authored changes and review comments were given high authority where they establish core API, conversation, TVBuff, and ABI semantics. Closed !7999 was treated as negative/submission-process evidence only and was not used as an accepted implementation exemplar.

## Next frontier

!7960 exists in this corpus snapshot, is merged on `release-4.0`, and is titled `DoIP: Prepare for ISO 13400-2:2019Amd1 and newer`. It was inspected only as a frontier probe and is **not** counted as reviewed.
