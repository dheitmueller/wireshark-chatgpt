# Wireshark Reassembly Conventions

This file records durable packet-reassembly conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Track bytes actually received, and expose a reassembly only when it is complete

A protocol's advertised final PDU/SDU length is not the amount of data present in an individual fragment. Reassembly bookkeeping must copy and advance by the bytes actually available in each fragment; attempting to copy the final aggregate length from the first fragment can trigger bounds failures and corrupt the reassembly state. Likewise, reaching a packet marked as the last fragment does not by itself prove that the aggregate buffer is complete when earlier data was missing.

Merged master MR !22271, authored and merged by John Thacker, fixes Bluetooth L2CAP I-frame reassembly so the first fragment copies only its actual `length` and initializes the accumulated offset from that length, rather than copying the advertised complete SDU length. The change was validated against an existing issue capture whose fragments subsequently reassemble into an OBEX packet. Release backports !22273 and !22274 carry the same fix.

Merged master MR !22283, also authored by John Thacker, adds the complementary completion invariant: the dissector creates and publishes a reassembled child TVB only when the accumulated offset exactly equals the expected total length (`cur_off == tot_len`). A nominal last fragment with a mismatch indicates missing data or other reassembly failure and must not be presented as a complete SDU. Release backports !22286 and !22287 reinforce that this was considered release-worthy correctness behavior.

**Reassembly rule:** distinguish expected aggregate length from bytes actually captured. Copy and advance state by real fragment lengths, and do not hand a reassembled buffer to downstream dissection until the bookkeeping proves the expected aggregate has actually been filled.

**Confidence:** Very high. Two adjacent merged master fixes authored by John Thacker, both backported to release branches; one is explicitly validated against a known failing capture.
