# Wireshark Capture Link-Layer Byte-Order Conventions

This file records durable conventions for byte ordering in captured link-layer and pseudo-header formats. Current wiretap/libpcap specifications and upstream source remain authoritative.

## Keep persistent capture formats independent of the producer host's native byte order

A capture link type should not acquire fields whose interpretation changes with the byte order of the machine that wrote or last processed the capture. Host-endian pseudo-header fields turn an otherwise portable capture format into one that requires file readers and remote-capture clients to know producer-host metadata or perform format-specific byte swapping.

Merged master MR !14320, authored and merged by Guy Harris, adds SocketCAN CAN XL support and explicitly treats the CAN XL fields in `LINKTYPE_CAN_SOCKETCAN` as little-endian. The rationale is to avoid making that link-layer type another host-endian format that requires special handling in pcap/pcapng readers and rpcap clients. Release-4.2 MR !14322 carries the corresponding support.

**Implementation rule:** when defining or extending an on-disk capture pseudo-header, use the canonical byte order specified for that encapsulation and keep it stable across producer architectures. Do not serialize an in-memory native-endian structure verbatim merely because the originating OS exposes it that way.

**Review rule:** distinguish an operating system's native in-memory capture structure from the persistent link-layer representation stored in pcap/pcapng. Verify that captures written on big- and little-endian hosts decode identically, and audit remote-capture paths so they do not need implicit knowledge of the source host's byte order.

**Confidence:** Extremely high. The accepted master implementation was authored and merged by Guy Harris, who explicitly documented the portability reason for the chosen persistent byte order; the release backport corroborates that contract.
