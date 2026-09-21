# Wireshark Tap-Data Contract Conventions

This file records durable conventions for values exported by dissectors to taps and consumed by statistics/analysis code. Current upstream source remains authoritative.

## Publish canonical normalized values once and reuse them in tap consumers

When a dissector has already converted a wire value into the semantic representation downstream analysis needs, export that canonical value through the tap contract rather than forcing every tap consumer to reconstruct it independently. Repeating normalization logic in the UI/statistics layer creates multiple subtly different interpretations of the same packet.

Merged master MR !14229, authored and merged by John Thacker, changes RTP Analysis to consume `info_extended_timestamp`, which the RTP dissector already calculates with wrap handling and places in the tap data. The analysis code had separately tried to extend the 32-bit RTP timestamp; its version contained subtle signed-versus-absolute-difference and casting problems. The accepted fix removes that duplicate state/calculation and uses the dissector-provided 64-bit extended timestamp throughout the statistics path. The MR was tested against multiple wrap/reordering edge cases referenced by earlier RTP issues/MRs.

**Architecture rule:** normalize a packet value at the layer that has authoritative protocol context, then carry that semantic value across the tap/API boundary. Downstream graphs, statistics, dialogs, and exporters should consume the normalized field instead of re-deriving it from the raw wire field unless they intentionally implement a different semantic operation.

**API rule:** if both raw and normalized forms are useful, name and type them distinctly in the tap structure so consumers cannot accidentally substitute one for the other. Prefer a representation wide enough to preserve the normalized domain rather than making each consumer repeat wrap-extension or sign-conversion policy.

**Testing rule:** when replacing duplicated downstream normalization with a canonical tap value, exercise wrap, reordering, boundary, and other cases that made the original reconstruction ambiguous. The objective is semantic equivalence across all consumers, not merely simpler code on ordinary captures.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker; the MR directly documents both the duplicate calculation and the edge cases that exposed its differences.