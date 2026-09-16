# Automated Wireshark MR review: !18909–!18958

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Review direction: descending from newest toward older MRs. Existing tracking in `reviewed-mrs-automation/` and `reviewed-mrs.md` was consulted before selection; the historical !17571–!17620 batch remains counted. This run records exactly 50 MRs and does not infer review coverage from numeric-range filenames.

## Exact reviewed set

!18958, !18957, !18956, !18955, !18954, !18953, !18952, !18951, !18950, !18949, !18948, !18947, !18946, !18945, !18944, !18943, !18942, !18941, !18940, !18939, !18938, !18937, !18936, !18935, !18934, !18933, !18932, !18931, !18930, !18929, !18928, !18927, !18926, !18925, !18924, !18923, !18922, !18921, !18920, !18919, !18918, !18917, !18916, !18915, !18914, !18913, !18912, !18911, !18910, !18909

## Review notes

Merged MRs were weighted above closed/abandoned/superseded work, and substantive maintainer feedback was weighted above mechanical approvals and system notes.

Notable durable evidence from this batch includes:

- !18930 (merged, John Thacker): QUIC STREAM reassembly moved away from packet addresses/ports and keys on QUIC connection identity, stream identity, direction, and fragment/reassembly identity. This is strong corroboration for the notebook's existing rule that reassembly/conversation keys must model protocol-semantic identity rather than convenient transport endpoint state, particularly for protocols supporting endpoint migration or nested dissectors that may alter `packet_info` endpoint fields.
- !18950 (merged release-4.2 fix, John Thacker): manual TCP sequence analysis can execute when timestamp calculation is disabled, so the required per-packet TCP analysis object must be created at that use site if the preference-dependent path did not already create it. This corroborates lifecycle guidance: optional/preference-controlled initialization paths cannot be assumed to have established state needed by independent features.
- !18940 (merged, John Thacker): restores `wtap_rec_cleanup()` after tshark's first pass after an earlier buffer refactor accidentally removed it. This reinforces explicit lifecycle symmetry and checking cleanup obligations when refactoring ownership-bearing objects.
- !18958 (merged): Thread Network Data recursive sub-TLV parsing propagates the current Server TLV decode mode through recursion rather than resetting it locally. This corroborates preserving caller/parser context through recursive dissection.
- !18920 is an automatic registry/translation update and provides little reusable engineering-review evidence.
- !18910 is a small merged PDU-Transport UAT-label typo correction and provides no additional durable convention.

No new convention file was added in this run because the strongest durable findings independently corroborate conventions already represented in the notebook rather than establishing a materially new rule.

## Continuation

Future descending review should rebuild the already-reviewed set from all tracking files before selection. If no higher-numbered gaps are found, continue below !18909.
