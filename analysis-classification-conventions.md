# Wireshark Analysis Classification Conventions

This file records durable rules for stateful packet-analysis classifications and their exposed analysis fields. Current upstream source remains authoritative.

## When a packet is reclassified, clear semantic states that the new classification supersedes

Analysis flags are user-visible semantic claims, not merely an append-only trace of predicates that happened to match while the algorithm ran. If two flags represent competing explanations of the same packet, correcting the classification should remove the superseded interpretation rather than leave contradictory states set and teach downstream display filters or color rules to compensate.

Merged master MR !13665 fixes TCP duplicate-ACK analysis when SACK information and a window change were present. John Thacker's substantive review explicitly asked whether a packet identified as a duplicate ACK *rather than* a Window Update should clear `TCP_A_WINDOW_UPDATE` instead of merely adding `TCP_A_DUPLICATE_ACK`; doing so also made the proposed color-filter workaround unnecessary. He separately caught redundant flag-setting and redundant filter logic. The accepted change was merged after that review.

**Implementation rule:** define which analysis classifications may legitimately coexist and which are alternatives. When later evidence selects one alternative, normalize the stored analysis state by clearing any incompatible earlier flag; do not expose both just because the code reached both predicates at different points.

**Architecture rule:** fix semantic classification at the analyzer that owns it. Avoid papering over inconsistent state in color filters, GUI presentation, taps, or other consumers, because those workarounds leave every other consumer observing the wrong model.

**Testing rule:** assert the complete expected set of analysis flags for ambiguous edge cases, including the *absence* of superseded flags. For TCP-style sequence analysis, also verify downstream retransmission classification, since one incorrect ACK interpretation can alter later packet analysis.

**Confidence:** Very high. Merged master TCP analysis correction with detailed John Thacker review focused directly on mutually exclusive semantic flags and downstream filter consequences.