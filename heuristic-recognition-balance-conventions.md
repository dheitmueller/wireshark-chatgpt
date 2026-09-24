# Heuristic Recognition Balance Conventions

This file records durable Wireshark conventions for balancing false-positive avoidance against the ability to dissect and debug malformed protocol traffic. Current upstream source remains authoritative.

## Strengthen heuristic recognition with invariants, but do not turn the heuristic into a full validity checker

Heuristic dissectors run on arbitrary traffic and therefore need enough cheap, protocol-specific evidence to avoid false positives. At the same time, every additional rejection condition excludes some malformed instances from the protocol dissector, which can make debugging broken implementations harder. Recognition should therefore use strong invariants that distinguish the protocol without demanding that the entire message already be valid.

Merged master MR !11092 adds two checks to the SAP HANA SQL Command Network Protocol path before invoking `tcp_dissect_pdus()`: the complete fixed header must be captured, and protocol-reserved header bytes must have their required zero values. The stated goal is to reduce false-positive dissector selection. The MR explicitly declines to add every possible validation check because increasing heuristic strictness would reduce the dissector's usefulness on malformed packets. Alexis La Goutte approved and merged the change.

**Recognition rule:** require enough captured bytes for every probe read, then prefer stable, inexpensive protocol invariants such as reserved constants, magic values, structural relationships, or constrained version fields to discriminate candidate traffic.

**Tolerance rule:** do not automatically promote every semantic validity condition into the heuristic. Once traffic has enough identifying evidence to belong to the protocol, let the normal dissector diagnose malformed fields where practical rather than rejecting the packet as “not this protocol.”

**Review rule:** for each proposed heuristic check, ask two questions separately: (1) how much does this reduce plausible false positives, and (2) what malformed-but-recognizable protocol traffic would it hide? The appropriate boundary is recognition, not full conformance validation.

**Confidence:** High. Merged master heuristic correction approved/merged by Alexis La Goutte, with the false-positive/debuggability tradeoff stated directly in the accepted MR rationale.
