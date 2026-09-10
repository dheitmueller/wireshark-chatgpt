# Wireshark Heuristic Dissector Conventions

This file records durable conventions for heuristic-dissector registration and default enablement extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Broad heuristics should default off when protocol metadata normally provides a stronger discriminator

A heuristic can be useful for captures in which the normal protocol discriminator is missing, but that does not imply it should run by default on every payload of the carrier protocol. When the payload syntax is generic enough to create a meaningful false-positive or performance surface, register the heuristic so users can opt in while leaving it disabled by default.

Merged MR !24231, authored and merged by John Thacker, adds JSON heuristic dissection to HTTP specifically for custom protocols that carry JSON without declaring an appropriate media type. The registration uses `HEURISTIC_DISABLE`, unlike narrower contexts where JSON heuristics are enabled. This preserves an escape hatch for underspecified traffic without making generic JSON probing part of normal HTTP dissection.

**Implementation rule:** prefer explicit media types, ports, dissector tables, or other protocol-owned discriminators when available. Add a broad heuristic as an opt-in fallback when it is valuable for nonconforming/underspecified traffic but not sufficiently selective for default use.

**Confidence:** Very high. Merged master change authored and merged by John Thacker, with the disabled-by-default behavior explicit in both the MR description and implementation.
