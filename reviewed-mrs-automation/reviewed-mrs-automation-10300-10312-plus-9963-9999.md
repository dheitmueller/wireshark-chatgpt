# Wireshark MR automation ledger — !10312–!10300 plus !9999–!9963

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This is the authoritative exact-set ledger for this run. Exactly 50 previously unreviewed MRs were selected by individual membership in the accumulated tracking. The gap !10299–!10000 is already covered by earlier per-run ledgers. The historical !17571–!17620 batch was rechecked and still contains 50 unique MRs.

| MR | Outcome |
|---|---|
| !10312 | merged |
| !10311 | merged |
| !10310 | merged |
| !10309 | merged |
| !10308 | merged |
| !10307 | merged |
| !10306 | merged |
| !10305 | merged |
| !10304 | merged |
| !10303 | merged |
| !10302 | merged |
| !10301 | merged |
| !10300 | merged |
| !9999 | merged |
| !9998 | merged |
| !9997 | merged |
| !9996 | merged |
| !9995 | merged |
| !9994 | merged |
| !9993 | merged |
| !9992 | merged |
| !9991 | merged |
| !9990 | merged |
| !9989 | merged |
| !9988 | merged |
| !9987 | merged |
| !9986 | merged |
| !9985 | merged |
| !9984 | merged |
| !9983 | merged |
| !9982 | merged |
| !9981 | merged |
| !9980 | merged |
| !9979 | merged |
| !9978 | merged |
| !9977 | closed / down-weighted |
| !9976 | closed draft / down-weighted |
| !9975 | merged |
| !9974 | merged |
| !9973 | merged |
| !9972 | merged |
| !9971 | merged |
| !9970 | merged |
| !9969 | merged |
| !9968 | merged |
| !9967 | merged |
| !9966 | merged |
| !9965 | merged |
| !9964 | merged |
| !9963 | merged |

## Exact reviewed MR set

!10312, !10311, !10310, !10309, !10308, !10307, !10306, !10305, !10304, !10303, !10302, !10301, !10300, !9999, !9998, !9997, !9996, !9995, !9994, !9993, !9992, !9991, !9990, !9989, !9988, !9987, !9986, !9985, !9984, !9983, !9982, !9981, !9980, !9979, !9978, !9977, !9976, !9975, !9974, !9973, !9972, !9971, !9970, !9969, !9968, !9967, !9966, !9965, !9964, !9963

Count: **50 unique MRs**. Highest: **!10312**. Lowest: **!9963**.

Merged: **48**. Closed/unmerged: **2** (!9977 and !9976).

Corpus artifact note: `mr_9976.json` is empty at this corpus commit. Its identity and state were reconstructed from the upstream Wireshark GitLab MR solely so it could be counted accurately. It was a closed draft targeting release-4.0 with a large unrelated diff footprint and no substantive human review, so it received essentially no architectural weight.

## Promoted or corroborating findings

- !9963 with !9964/!9965: failure-only error outputs must not be consumed after a successful operation.
- !9991: a heuristic TRUE return is a protocol-ownership claim and must follow positive recognition.
- !9996: complete SOME/IP-TP reassembly identity requires both transport and protocol-level identifiers.
- !9999: check finite state-machine stack transitions before mutating depth and centralize transition failure state.
- !10301: pass child dissectors a tvbuff bounded to the semantic structure already delimited by the parent.
- !9977: direct release-policy corroboration that new features are not backported to stable branches.

Full notes: `review-findings-9963-10312-noncontiguous.md`.

## Next frontier

MR !9962 (`TECMP: cleanup endianness for 1 Byte fields`) exists at the same corpus commit and is merged. It was inspected only for metadata to establish the next frontier and was not reviewed or counted.
