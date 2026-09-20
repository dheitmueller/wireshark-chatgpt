# Wireshark Flow-Graph Layout Conventions

This file records durable conventions for arranging bidirectional endpoints in Wireshark sequence and flow visualizations. Current upstream source remains authoritative.

## Keep endpoint positions stable and encode packet direction with the arrow

A bidirectional visualization should not choose left-versus-right placement from the source/destination role of each individual packet. If both directions of one logical conversation can swap endpoint positions from row to row, the graph becomes harder to follow and same-address conversations can become ambiguous.

Merged master MR !15245 fixes Flow Graph and VoIP sequence handling for conversations whose source and destination addresses are identical but whose ports differ. During review, John Thacker specifically suggested placing the smaller of the two ports on the left instead of always placing the source node there, so the two endpoints remain in one relative order and direction is represented by the arrow reversing. The accepted revision also corrected a source/destination node mix-up found in that review.

**Implementation rule:** derive the visual ordering of a pair of endpoints from a stable, direction-independent endpoint key (for example, address plus port, with a deterministic tie-breaker). Keep that ordering consistent for the logical pair across packets, and use arrow orientation or equivalent decoration to show the packet direction.

**Review implication:** test both directions of a conversation, including cases where addresses are identical and only ports or another endpoint discriminator differ. A layout that looks correct for one direction can still swap or collapse nodes when traffic reverses.

**Confidence:** Very high. Merged master UI correctness fix with explicit John Thacker review shaping the accepted layout rule.