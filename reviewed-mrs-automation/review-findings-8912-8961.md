# Wireshark MR Review Findings: !8912-!8961

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly 50 previously unreviewed MRs in descending order, !8961 through !8912. There are 46 merged MRs and four closed/unmerged MRs: !8958, !8953, !8926, and !8923. The exact set is recorded in `reviewed-mrs-automation-8912-8961.md`.

## Strongest durable findings

- !8937: cached first-gap/contiguous reassembly state produced a major performance improvement, but the cached state must remain synchronized with every change to the fragment list. John Thacker stress-tested the accepted implementation.
- !8931: a currently complete-looking transfer still needs reassembly identity when the protocol allows a later duplicate.
- !8917: Pascal Quantin explicitly distinguished ordered first-pass state construction from arbitrary later redissection; first-pass-only state changes should be visibly confined to that pass.
- !8933, !8946, and !8919: keep complete semantic string values separate from UI-label and lookup limits.
- !8948, together with the already-reviewed correction !8962: a failure status and its structured diagnostic must have a complete, consistent contract.
- !8955 and !8959: asn2wrs source-mapping directives are a common opt-in debugging mode rather than normal per-dissector output.

## Qualified non-merged evidence

Closed !8926 is not accepted implementation evidence, but Guy Harris's review is architecturally useful: dissector return values can represent several different kinds of outcome, so adding more special integer meanings to a byte-count return deserves caution and a cleaner contract may be preferable.

Closed !8923 is also down-weighted. The proposed separate bounded UTF-8 buffer did not merge; the later accepted direction in !8946 and !8964 removed the hidden generic label-size limit instead.

## Corroborating review evidence

!8951 supplies another example of Alexis La Goutte requesting a focused capture before accepting a parser fix. !8956 and !8957 show source-checker improvements validated against real code. !8954 reinforces checking system API status before passing results onward. !8929 and !8924 show static-analysis and length-semantics issues being caught during review. !8913 shows John Thacker finding the same offset defect in a sibling path before merge, followed by the explicit stable backports !8920 and !8921.
