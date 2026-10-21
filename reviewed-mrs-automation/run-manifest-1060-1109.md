# Run manifest: Wireshark MR review !1060-!1109

- Model: GPT-5.6 Sol
- Corpus: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Direction: newest available unreviewed MRs toward older MRs
- Selection: exactly 50 highest-numbered corpus MRs absent from the reconciled reviewed set
- Exact set: !1109 through !1060 inclusive, with each candidate checked individually rather than assuming range coverage
- Outcomes: 49 merged; !1076 closed/unmerged and superseded by merged !1077
- Tracking consulted: `reviewed-mrs.md`, aggregate automation tracking, exact low-number per-run ledgers through !1110-!1159, and the dedicated !17571-!17620 ledger; candidate exact-number searches were also run against the available notebook tracking
- Historical !17571-!17620 preservation check: exactly 50 unique MRs
- Weighting: merged MRs > closed/superseded work; direct maintainer guidance and maintainer-authored MRs weighted strongly, especially Guy Harris
- ST 291/VANC encountered: no
- Next frontier probe only: !1059, `GMR-1 RR: Use tvbuff_new_octet_aligned to get octet aligned tvbuff` (merged, master, John Thacker)
- Corpus exhausted: no
