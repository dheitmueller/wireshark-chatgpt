# Review findings: Wireshark MRs !7911-!7960

Corpus: `dheitmueller/wireshark-corpus-mrs@ddcaa22b51c68f594e425a23388c3a2086813054`

All 50 MRs in the exact ledger were reviewed. 49 are merged and !7939 is closed.

Strong findings:
- Guy Harris's !7931 and !7934 distinguish conversations, endpoints, and conversation-element state; !7915 rejects type-punning between independent enum domains.
- John Thacker's !7949 separates sender-side TCP retransmission analysis from capture-side reassembler old-data state.
- !7926 uses CTest fixtures to make unit-test build prerequisites explicit; !7941 is the release-4.0 backport.
- !7922 scopes NetFlow/IPFIX sequence history to Observation Domain within a Transport Session and stores per-frame conclusions in packet proto-data.
- Pascal Quantin's !7948 review reinforces editing authoritative source data rather than generated output.
- !7954 centralizes graceful-shutdown signal handling for C extcaps.
- !7940 continues analyzer validation after TCP option EOL to report non-zero padding.
- !7960 preserves bounded visibility for a compatible but unknown DoIP version instead of stopping all dissection.

Backports and routine dependency/data updates were treated mainly as corroboration. Closed !7939 was not used as an accepted implementation exemplar.
