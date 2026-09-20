# Wireshark Sequence-Number Conventions

This file records durable conventions for wrapped and extended protocol sequence numbers extracted from accepted Wireshark review. Current upstream source remains authoritative.

## Reuse the canonical extended sequence representation instead of independently counting wraps

When a lower protocol layer already derives an extended sequence number from a wrapping wire field, downstream taps, statistics, and reassembly consumers should use that canonical representation rather than implement another wrap/cycle counter. Independent wrap logic is especially fragile in the presence of reordered, duplicated, or missing packets because a late packet can cross the wrap boundary again without representing a new cycle.

Merged master MR !16146, authored by John Thacker and merged by Anders Broman, fixes RTP analysis statistics that maintained their own 16-bit wrap-cycle state. Out-of-order traffic near sequence zero could increment that cycle count more than once. The accepted implementation uses the RTP dissector's existing `info_extended_seq_num`, stores the start/stop sequence range in that extended domain, removes the duplicate `under`/`seq_cycles` state, and computes expected packet count directly from the extended range.

**Implementation rule:** establish one authoritative wrap-extension algorithm at the layer that understands the protocol sequence semantics, then pass or expose the resulting extended value to downstream analysis. Do not recreate cycle tracking in each consumer from the raw wrapped field.

**Testing rule:** exercise wrap boundaries together with reordering, loss, and duplication. A test that only sends monotonic in-order values through a wrap cannot expose false extra cycles caused by late packets crossing the boundary again.

Merged MR !16261 provides corroborating negative evidence: a later John Thacker note records a TCP analysis regression at 32-bit sequence-number wraparound, reinforcing that apparently ordinary arithmetic on sequence values must be reviewed and tested as modular/extended-sequence arithmetic rather than as unbounded integers.

**Confidence:** Very high for the canonical-extended-value rule: merged master correctness fix authored by John Thacker and merged by Anders Broman, with the failure mode and replacement model explicit in the accepted diff. High for the general wraparound testing rule, additionally corroborated by the later TCP wraparound regression note.
