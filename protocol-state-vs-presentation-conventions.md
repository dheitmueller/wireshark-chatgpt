# Protocol state versus presentation conventions

## Keep wire identity separate from presentation-normalized sequence numbers

Merged master MR !6079, authored by John Thacker, fixes SCTP relative TSN handling by preserving the raw TSN independently from the displayed relative TSN. The first observed DATA TSN initializes the association's relative-display base, while retransmission tracking receives the raw wire TSN. Stable MR !6081 carries the same fix.

**Implementation rule:** values transformed for presentation—relative numbering, normalization, formatting, or offsets from a display base—must not replace the raw value used for protocol-state identity unless the protocol itself defines the transformed domain. Preserve both representations when stateful analysis and presentation need different values.

**Testing rule:** when adding relative sequence display, verify first-pass behavior and retransmission detection independently so display normalization cannot perturb protocol history.

**Confidence:** Extremely high. Merged master correctness fix authored by John Thacker with an accepted stable backport.
