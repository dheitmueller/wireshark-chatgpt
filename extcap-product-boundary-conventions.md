# Wireshark Extcap Product-Boundary Conventions

This file records durable conventions for deciding which Wireshark-family product owns an extcap. Current upstream packaging and product architecture remain authoritative.

## Classify an extcap by the semantic data it produces, not merely by the operating-system API it uses

An extcap's implementation technology or source subsystem does not by itself determine whether it belongs with Wireshark or Stratoshark. The important product boundary is the kind of capture data the extcap exposes to the user.

Merged master MR !23550 moved Windows system-event extcaps toward Stratoshark while retaining `etwdump` with Wireshark. During review Gerald Combs explicitly corrected the proposed move of `etwdump`: although it uses Windows ETW infrastructure, it emits network packets, so it belongs with Wireshark. The accepted MR was revised accordingly before merge.

**Implementation rule:** when assigning or moving an extcap between Wireshark-family products, classify it by the semantics of its emitted records. Network-packet capture sources belong with Wireshark even when their acquisition mechanism is also used for system-event tracing; event/log/system-activity sources may belong with Stratoshark. Audit packaging, installation, and frontend exposure against that semantic boundary.

**Confidence:** Very high. Merged master architecture/packaging change with direct Gerald Combs review that changed the proposed product assignment.