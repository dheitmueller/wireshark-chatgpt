# Review findings: Wireshark MRs !9913-!9962

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

Reviewed-set reconstruction used the available per-run ledgers in reviewed-mrs-automation/ plus reviewed-mrs.md on automation/mr-review-9963-10312-noncontiguous. The historical !17571-!17620 ledger was rechecked and contains all 50 unique MRs. The selected batch is exactly !9962 through !9913: 49 merged MRs and one closed/unmerged MR (!9930).

## Promoted findings

- **!9950, merged, John Thacker:** TLS renegotiation can change the conversation's selected cipher. Key Exchange redissection therefore snapshots the cipher in packet data during the first pass and restores that packet-specific value later. Added to dissector-state-conventions.md.
- **!9957, merged, John Thacker:** Follow Stream UI is generated from registered followers/protocol IDs rather than a fixed enum and hard-coded action list, allowing plugins to participate without central UI edits. Chuck Craft's empty-submenu review was resolved by disabling the empty dynamic action. Added to registration-extension-point-conventions.md.
- **!9925, merged, John Thacker:** negative bit lengths from -1 through -7 could round to a zero-byte length before ordinary bounds checks. The core proto API now rejects the signed invalid domain before conversion. Added to api-precondition-validation-conventions.md.
- **!9921, merged, John Thacker:** temporary RTP lookup IDs shallow-copy packet addresses; retained stream IDs deep-copy them. Added as corroboration to address-copy-lifetime-conventions.md.
- **!9919, merged:** Sharkd rtp-streams emitted SSRC in decimal while rtp-analyse/rtp-download tokens expected hexadecimal. The producer and consumer formats were aligned and self-tests plus a capture were added. Gilbert Ramirez also requested documentation for Sharkd-specific error codes. Added to machine-output-conventions.md.
- **!9914, merged, John Thacker:** restoring conversation elements after every dissector call erased final packet conversation/address state used by post-dissection consumers. The revert distinguishes nested-call working state from final per-packet state. Added to dissector-entry-and-state-conventions.md.
- **!9930, closed:** Alexis La Goutte rejected direct edits to generated DCE/RPC output and directed the work to srvsvc.idl/cnf; the author opened !10009 as the source-side replacement. Kept only as negative/generated-source corroboration.
- **!9953, merged:** John Thacker requested the functional fix be applied to both Wireshark and Logray and later requested a clean local rebase/squash after the branch history confused GitLab's rebasing. Useful submission and cross-frontend corroboration.
- **!9924, merged:** John Thacker checked the DRDA SQL-length interpretation against EBCDIC captures before accepting the field split, reinforcing protocol-variant validation rather than applying a layout assumption globally.
- **!9918, merged:** Sharkd now returns a result status for a readable but truncated capture instead of leaving the RPC client without a response; a focused truncated-pcap regression test was added.

## Remaining scanned MRs

The other 40 MRs were inspected for metadata, final diff, and human discussion where present. They consist primarily of release backports/build preparation, targeted Qt/RTP leak fixes, documentation/help alignment, static-analysis cleanup, small dissector field/presentation fixes, and automatic data updates. They did not justify new durable notebook rules beyond the promoted/corroborating findings above.

Exact membership and the corpus commit are recorded in reviewed-mrs-automation/reviewed-mrs-automation-9913-9962.md.

The corpus is not exhausted. !9912, "Qt: have_field_info superseded by have_packet_bytes," exists and is merged; it was checked only as the next frontier and is not part of this run.
