# Wireshark Dissector Return-Value Conventions

This file records durable conventions for the byte-count values returned by Wireshark dissectors. Current upstream source remains authoritative.

## Return the bytes that are actually present and owned by the dissector

A dissector return value should describe the bytes from the supplied tvbuff that the dissector claims, not bytes that would have existed in an unsliced packet or a protocol-declared length that is not actually captured.

When the entire supplied tvbuff belongs to the protocol — including trailing event data, frame data, padding, or other bytes that are still part of that protocol's record — return `tvb_captured_length(tvb)`. Do not claim bytes beyond the captured tvbuff merely because the original packet was longer before snaplen slicing.

**Evidence:** merged master MR !20593, authored and merged by Guy Harris, changed the LIN dissector to return `tvb_captured_length(tvb)`. Guy's rationale is explicit: all bytes present in that tvbuff belong to LIN, including bytes after the event message/frame data and padding; bytes absent because the packet was sliced obviously cannot be claimed. The same fix was immediately backported and merged as !20594 (release-4.4) and !20595 (release-4.2).

**Review rule:** when a dissector returns a byte count, distinguish three quantities that can differ: protocol-declared/reported length, captured tvbuff length, and the subset actually claimed by the dissector. Use the quantity that matches the API contract. In particular, do not make truncation look like successful consumption of bytes that are not present.

**Confidence:** Very high. The master change and both stable backports were authored/merged by Guy Harris, and the rationale directly states the intended semantics.
