# Reassembly Late-Duplicate Conventions

Merged MR !8931 shows that a transfer should remain associated with reassembly state when the protocol allows a later duplicate.

**Rule:** decide whether reassembly can be skipped from protocol delivery semantics, not only from the number of fragments currently observed.

**Confidence:** Very high; merged core and USBLL work.
