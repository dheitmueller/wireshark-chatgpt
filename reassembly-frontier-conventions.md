# Wireshark Reassembly Frontier Conventions

Merged master MR !7897, authored by John Thacker, fixes TCP out-of-order multi-segment-PDU tracking. When a new fragment fills a gap inside an in-progress PDU, fragments already buffered after that gap may immediately become contiguous too.

**Rule:** a highest-contiguous-sequence value is a property of the complete stored fragment set, not just the latest fragment. After filling a gap, advance through every now-contiguous fragment before recording the new frontier.

**Testing rule:** cover insertion into the middle of an in-progress reassembly where one arriving fragment makes multiple previously buffered fragments contiguous.

**Confidence:** Very high. Merged master TCP correctness fix authored by John Thacker.
