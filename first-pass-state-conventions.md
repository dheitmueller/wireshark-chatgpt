# First-Pass State Conventions

Merged MR !8917 includes Pascal Quantin review explaining that the initial dissection pass is ordered by frame number while later redissection can occur in arbitrary order.

**Rule:** state changes that are valid only while building sequential capture history should be explicitly guarded at the state-change site. Do not rely on distant control flow to make later redissection harmless.

**Confidence:** Very high; merged fix with direct maintainer review.

## Protocol-tree shape should not depend on whether an incomplete PDU happened to be visited on the first pass

Merged master MR !6353, authored by John Thacker, fixes HTTP/2 so protocol-column and protocol-tree creation occur inside the complete-PDU callback used by `tcp_dissect_pdus()`. The previous code could add an empty HTTP/2 layer on the initial pass even when not enough bytes existed to dissect a PDU; later passes would skip that call, producing a different layer stack.

**Rule:** when a framing helper decides whether a complete PDU exists, defer protocol-layer creation and other message-specific side effects until the complete-PDU callback. First-pass and redissection tree/layer structure should agree for the same logical data.

**Confidence:** Very high. Merged master correctness fix authored by John Thacker.
