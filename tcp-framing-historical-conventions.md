# Historical TCP Framing Conventions

## E1AP: use the stream-PDU helper for length-delimited TCP protocols

Merged master MR !1768 initially added E1AP-over-TCP by stripping a four-byte length indication and calling the PDU decoder directly. Pascal Quantin explicitly pointed out that TCP is stream-oriented and that the implementation would fail once a PDU spans more than one segment. He then authored merged !1771, which uses a four-byte minimum header, a PDU-length callback, and `tcp_dissect_pdus()`.

**Rule:** if a TCP-carried protocol provides deterministic PDU framing, let `tcp_dissect_pdus()` own segmentation, coalescing, and repeated-PDU iteration rather than assuming TCP segment boundaries match application PDU boundaries.

This is historical corroboration for the notebook's existing transport-framing guidance rather than a competing convention.

**Confidence:** Extremely high. Direct Pascal Quantin review followed by his merged corrective implementation.
