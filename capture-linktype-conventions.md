# Capture Link-Layer Type Conventions

This file records durable Wireshark conventions for handling libpcap DLT values and capture-file LINKTYPE values. Current Wiretap/libpcap behavior and upstream source remain authoritative.

## Normalize host-specific DLT values before writing portable capture metadata

A libpcap `DLT_*` value is not automatically a portable capture-file `LINKTYPE_*` value. Most numeric values happen to coincide, but some differ by operating system or were historically unavailable on particular systems. `LINKTYPE_*` exists to provide a single file-format value for a link-layer type independent of the host on which the capture was made.

Merged master MR !11672, authored by Guy Harris, changes dumpcap to map link-layer types obtained from libpcap to the corresponding `LINKTYPE_*` value before placing them in a pcap header or pcapng Interface Description Block. Guy's description also points out that a piped pcap file can come from a producer whose libpcap did not perform that normalization, so dumpcap normalizes that boundary too. Merged release-4.0 backport !11673 preserves the same behavior.

**Implementation rule:** treat `DLT_*` as a host/API-domain identifier and `LINKTYPE_*` as the portable capture-file identifier. Convert explicitly at the boundary where a libpcap/native value becomes file metadata; do not rely on the common-but-not-universal equality of their numeric values.

**Interoperability rule:** when ingesting externally produced pcap data, do not assume every producer has already performed the host-specific DLT-to-LINKTYPE conversion. Normalize where Wireshark takes ownership of the portable file representation.

**Review rule:** whenever code copies a link-layer type from libpcap or a platform capture API into pcap/pcapng metadata, verify which namespace the value belongs to and whether a conversion is required. Numeric equality on the developer's current OS is not sufficient evidence of portability.

**Confidence:** Extremely high. Merged master change and stable backport authored by Guy Harris with an explicit historical and cross-platform rationale.