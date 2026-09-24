# Automated MR review ledger: !10613–!10662

Reviewed using GPT-5.6 Sol.

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Exactly 50 previously unreviewed merge requests were reviewed. The review-tracking inventory, `reviewed-mrs.md`, aggregate automation tracking, and per-run ledgers were consulted before selection. Candidate membership was checked individually rather than inferred from numeric ranges; none of !10613–!10662 appeared in prior tracking, while the immediately newer exact ledger starts at !10663. The historical !17571–!17620 ledger was re-opened and its 50-row table remains preserved and counted.

47 MRs in this batch are merged. !10660, !10633, and !10629 are closed/unmerged and were down-weighted. `mr_10629.json` is present but empty in this corpus commit; upstream GitLab was used only to reconstruct that MR. It was an accidental wrong-target backport immediately closed by Guy Harris and contributes no accepted convention.

## Exact reviewed set

- !10662
- !10661
- !10660
- !10659
- !10658
- !10657
- !10656
- !10655
- !10654
- !10653
- !10652
- !10651
- !10650
- !10649
- !10648
- !10647
- !10646
- !10645
- !10644
- !10643
- !10642
- !10641
- !10640
- !10639
- !10638
- !10637
- !10636
- !10635
- !10634
- !10633
- !10632
- !10631
- !10630
- !10629
- !10628
- !10627
- !10626
- !10625
- !10624
- !10623
- !10622
- !10621
- !10620
- !10619
- !10618
- !10617
- !10616
- !10615
- !10614
- !10613

## Promoted findings

- !10645, corroborated by !10661/!10662: preserve real pcapng IDBs and remap source-local interface IDs when combining capture sources.
- !10641: reassembled objects can be referenced by multiple table keys; shared result lifetime requires explicit reference ownership.
- !10626, corroborated by !10657: establish a string field's exact byte extent before constructing its typed value.
- !10620, corroborated by !10623/!10624: timestamp edits invalidate file-scoped analysis and require redissection.
- !10625 with !10628/!10630 and !10617 with !10619/!10621: Wiretap helpers need structured error returns and untrusted file-declared lengths must be validated before bounded copies.
- !10618: completed capture-file formats belong in Wiretap rather than extcap acquisition.
- !10637/!10650/!10632, !10655, !10651, and !10614 were retained as corroboration of existing checker, buffer-capacity, parser-boundary, and fixed-header guidance rather than duplicate conventions.

The corpus is not exhausted. !10612 exists at the same corpus commit and is merged; it was inspected only as the next-frontier probe and was not reviewed or counted.
