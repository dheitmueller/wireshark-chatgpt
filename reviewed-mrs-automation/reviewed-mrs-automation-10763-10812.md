# Wireshark MR automation review: !10812 through !10763

Corpus repository: dheitmueller/wireshark-corpus-mrs
Corpus commit reviewed: ddcaa22b51c68f594e425a23388c3a2086813054
Notebook commit immediately before this ledger: 6c64164817d5c47b258c0bb7ff5115e6770696a4
Review direction: descending from newest available previously-unreviewed MR.
Exact reviewed count: 50
State summary: 49 merged; 1 open/draft (!10792).

## Selection and review tracking

Before selecting this batch, the available review tracking in reviewed-mrs.md and reviewed-mrs-automation/ was consulted, including the immediately preceding complete per-run ledger for !10862 through !10813. The historical !17571 through !17620 batch was checked directly and all 50 entries remain explicitly represented. The previous mention of !10812 was only a frontier check and did not count as review.

Selection was based on individual MR-number membership rather than assuming numeric-range coverage. None of !10812 through !10763 was already present in the accumulated reviewed set, and all 50 MRs exist in the pinned corpus commit, so this is the exact next batch.

## Exact reviewed MR set

!10812
!10811
!10810
!10809
!10808
!10807
!10806
!10805
!10804
!10803
!10802
!10801
!10800
!10799
!10798
!10797
!10796
!10795
!10794
!10793
!10792
!10791
!10790
!10789
!10788
!10787
!10786
!10785
!10784
!10783
!10782
!10781
!10780
!10779
!10778
!10777
!10776
!10775
!10774
!10773
!10772
!10771
!10770
!10769
!10768
!10767
!10766
!10765
!10764
!10763

## Durable findings

- !10808, merged and authored by John Thacker: arithmetic width must be established at the operation itself, and variable-length integer parsers need an independent limit on encoded component count rather than relying only on the accumulated value.
- !10806, merged and authored by John Thacker: when a request selects the grammar of later responses, persist that mode explicitly and preserve enough first-pass/per-frame state for deterministic random-access redissection instead of guessing from ambiguous response bytes.
- !10784, merged and authored by John Thacker: the encoded byte length of packet text and the byte length of its converted representation are distinct; downstream storage must use the converted representation's actual length.
- !10782, merged and authored by John Thacker: avoid an unnecessary fixed-size formatting intermediate when the destination API can accept the original formatting inputs directly; the extra truncation boundary can damage multibyte text.
- !10780, merged, authored by John Thacker and merged by Gerald Combs: regenerate ASN.1 dissectors from canonical inputs in Code Checks and fail on a resulting repository diff. This is direct historical corroboration of the notebook's generated-source parity rule.
- !10765, merged after detailed Martin Mathieson review: corroborates representative capture expectations, project assertions, consistent field/filter naming, value mappings, and readable descriptions for specification abbreviations.
- !10797 and !10800 independently reinforce reviewer requests for representative capture material for protocol changes.
- !10770, with the matching stable-branch changes !10769 and !10766, bounds the QUIC Follow tap to the exact logical stream-data region and updates the multistream regression test accordingly.
- !10792 remains an open draft and was down-weighted. John Thacker explicitly chose to narrow the broader Media Type Export Objects proposal; the accepted HTTP/2 portion is represented by merged !10802.
- !10795 is merged packaging work but carries a later report that it did not work as expected; it is retained as cautionary context rather than promoted as a convention.

The durable convention summary for this run is stored in review-findings-10763-10812.md.

## Frontier

!10762, "MySQL: Make random access dissection work", exists in the same corpus commit and is merged. It was inspected only to establish the next descending frontier and is not counted among these 50 reviewed MRs.
