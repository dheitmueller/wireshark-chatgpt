# Wireshark MR review ledger — backfill !15376, !701, !261

Model: GPT-5.6 Sol
Corpus: dheitmueller/wireshark-corpus-mrs
Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054
Reviewed count: 3
Exact reviewed set: !15376, !701, !261
Outcome: all three are closed / unmerged and are down-weighted as implementation evidence.
SMPTE ST 291 / VANC encountered: no.

Selection reconciliation:
The current corpus tree was reconciled against available review tracking across the notebook review branches. The historical !17571-!17620 ledger still contains exactly 50 unique reviewed MRs.

!15376, !701, and !261 had previously been skipped because normal file-content reads returned empty bodies. The recursive Git tree at the pinned corpus commit shows all three paths backed by large non-empty blobs, and direct blob reads yield complete MR JSON. Their apparent emptiness was a retrieval-layer artifact.

After adding these three exact MR numbers, no other present corpus MR lacked completed-review tracking. The current corpus has no records for !8696, !9604, !16948, !21132, or !21133.
