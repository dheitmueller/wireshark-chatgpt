# Wireshark Capture Offload Conventions

This file records durable conventions for interpreting host/network-offload artifacts visible in packet captures. Current upstream source remains authoritative.

## Recognize known offload artifacts instead of reporting them as wire corruption

Packet bytes captured on a host can reflect an intermediate state before the NIC or driver has completed transmit offload work. When the operating-system offload mechanism leaves a well-defined structural signature, Wireshark should recognize that signature and explain it rather than presenting the packet as simply malformed or as having an ordinary bad checksum.

Merged master work represented by !13272 and its associated master change teaches TCP and UDP checksum verification about partial pseudo-header checksums used by local checksum offload on Linux and Windows. Wireshark computes the intermediate partial sum it already has while calculating the full Internet checksum; when the packet checksum field matches that expected partial value, the dissector reports an informative partial-checksum/offload expert condition rather than the ordinary bad-checksum path. The change also updates tests because an HTTP/2 reassembly example no longer appears malformed once the capture-host artifact is recognized.

Merged documentation MR !13275 received detailed review from Peter Wu. The accepted documentation emphasizes that such partial checksums are expected in packets generated/transmitted by the capture point and ties the behavior to segmentation/checksum-offload mechanisms rather than implying that the value is valid on the final wire.

**Implementation rule:** before classifying a checksum or framing anomaly as corruption, determine whether a supported capture point can expose a documented pre-offload representation. Recognize offload only from a specific, reproducible signature; do not broadly suppress checksum failures merely because offload is possible.

**Presentation rule:** distinguish “matches a known local offload intermediate value” from “checksum verified on the wire.” Explain the artifact in expert/output text so users can understand why locally captured transmit packets differ from packets observed after offload completion.

**Testing rule:** include host-side captures that exercise the offload representation and ordinary corrupted/nonmatching packets so recognition does not weaken real checksum diagnostics.

**Confidence:** Very high. Merged implementation and documentation changes by experienced maintainers, including a concrete mathematical signature for the intermediate checksum and explicit review of where the artifact can appear.