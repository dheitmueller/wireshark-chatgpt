# Review findings: Wireshark MRs 3861–3910

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged work is primary implementation evidence. Closed and open work is down-weighted.

| MR | Outcome | Depth | Finding |
|---:|---|---|---|
| 3910 | merged | scanned | ORAN Ext12 subtree display cleanup; no new convention. |
| 3909 | merged | scanned | Documentation spelling cleanup. |
| 3908 | merged | deep | Guy Harris: move serialized Exported-PDU definitions to wsutil; keep file-format values independent from mutable epan enums. |
| 3907 | merged | scanned | Stable backport of 3899. |
| 3906 | merged | scanned | Stable backport of 3898. |
| 3905 | merged | deep | Guy Harris: use phton16/phton32 helpers for Exported-PDU serialization instead of manual byte shifts. |
| 3904 | merged | deep | Guy Harris: pool-owned wmem storage must not be released through the default allocation domain. |
| 3903 | closed | discussion | Roland Knall: do not vendor/relicense SQLite; use normal optional dependency discovery; review packet-model and lifetime assumptions. |
| 3902 | merged | scanned | MySQL response-state correction. |
| 3901 | merged | deep | Guy Harris: evaluate random access/backward navigation for compressed captures, not only sequential codec throughput; Gerald Combs exposed portability failures. |
| 3900 | merged | discussion | Jaap Keuter: boolean field has no byte endianness; use ENC_NA. |
| 3899 | merged | scanned | Master-origin gsm_sim status-column fix. |
| 3898 | merged | scanned | Master-origin CoAP option interpretation fix. |
| 3897 | merged | discussion | Pascal Quantin: preserve removed preference keys as obsolete so existing profiles do not warn. |
| 3896 | merged | deep | John Thacker: distinguish complete PDUs that never entered reassembly from dangling incomplete fragments; validate one-pass and two-pass behavior. |
| 3895 | closed | discussion | Proposed padding fix withdrawn after bad example; Guy Harris and Pascal Quantin clarified TLV length/padding; 3919 is accepted documentation. |
| 3894 | merged | scanned | DoQ draft update. |
| 3893 | merged | deep | Explicit pinfo->pool migration including authoritative ASN.1 templates/configuration. |
| 3892 | merged | scanned | RTP CSV header usability change. |
| 3891 | closed | discussion | Plugin transitive-dependency discussion; merged 3945 is stronger accepted precedent. |
| 3890 | merged | deep | Pascal Quantin: use version-correct masks while retaining the same display-filter abbreviation for compatibility. |
| 3889 | merged | scanned | X11 GenericEvent length fix. |
| 3888 | merged | scanned | ITS ASN.1 presentation change. |
| 3887 | merged | scanned | MySQL EOF decoding correction. |
| 3886 | merged | discussion | Review favors add-and-return field APIs, offset-derived length expressions, spec-grounded labels, and a focused capture. |
| 3885 | open | discussion | Jaap Keuter/Pascal Quantin: TCP is a stream; use tcp_dissect_pdus around the protocol length field. Provisional because unmerged. |
| 3884 | merged | scanned | Automatic data update. |
| 3883 | merged | scanned | Automatic data update. |
| 3882 | merged | scanned | Automatic data/translation update. |
| 3881 | merged | scanned | Windows dependency update. |
| 3880 | merged | deep | Martin Mathieson adds typed-item checker coverage for unbalanced field-label punctuation and fixes findings. |
| 3879 | merged | discussion | Anders Broman enforces component-prefixed concise subject, blank line, readable paragraphs, and short lines. |
| 3878 | merged | scanned | NTLMSSP decryption cleanup. |
| 3877 | merged | scanned | NTLMSSP decryption behavior fix. |
| 3876 | merged | scanned | Signal PDU performance cleanup. |
| 3875 | merged | scanned | Signal PDU column-output fix. |
| 3874 | merged | scanned | LWAPP preference text correction. |
| 3873 | merged | deep | New dissector review requires filterable fields, sample/fuzz validation, and strict parser forward progress; field metadata mismatch also caused a registration crash. |
| 3872 | merged | scanned | ISO10681 transport dissector integration. |
| 3871 | merged | discussion | Guy Harris asks how a decimal-to-hex UAT notation change affects existing saved profiles. |
| 3870 | merged | scanned | CIP Motion field expansion. |
| 3869 | merged | scanned | 802.11 protocol extension. |
| 3868 | merged | scanned | WebSocket compression extension. |
| 3867 | merged | scanned | TECMP protocol correction. |
| 3866 | merged | discussion | Uli Heilmeier/Graham Bloice enforce issue linkage, component subject, squashed history, and valid author identity. |
| 3865 | merged | scanned | Qt packet-comment feature. |
| 3864 | merged | scanned | Project-governance documentation. |
| 3863 | closed | scanned | Closed iWARP attempt; merged 3976 is stronger conversation-identity precedent. |
| 3862 | merged | scanned | CMake module include fix. |
| 3861 | merged | scanned | Documentation-only stable fix. |
