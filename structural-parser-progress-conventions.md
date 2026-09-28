# Structural Parser Progress Conventions

This file records parser-progress lessons from merged MRs !5885, !5862, and !5863.

## Variable-length records must guarantee forward progress

Fuzzing found BLF records whose declared lengths could leave the reader at the same position. Merged !5885 rejects a log-container header shorter than the fixed base header and advances to the next object by at least the structural minimum, while also respecting a larger declared header length.

Merged !5862 and !5863 apply the same discipline to OpenFlow length-delimited structures. Before subtracting a fixed header size from a packet-controlled length, the dissector verifies that the declared length actually exceeds that fixed portion. Malformed entries receive expert information and terminate the containing parse rather than underflowing or looping.

**Rule:** every loop-driving record or item length must both cover the structure already consumed and produce a next cursor strictly beyond the current one. If malformed input violates either property, stop or skip using an explicit structural bound rather than trusting the declared length.

**Confidence:** High. All three changes merged; !5885 was driven by fuzz-discovered hangs and !5862 received reviewer follow-up across multiple vulnerable sites.
