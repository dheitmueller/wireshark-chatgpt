# Wireshark pcap Linktype-Metadata Conventions

## Decode the pcap network word as a compound field

Merged master MRs !6362 and !6364 are authored by Guy Harris. They establish that the pcap header's network value contains several semantic subfields, not one unconstrained integer.

The low 16 bits are the link-layer type so the same LINKTYPE value remains usable by pcapng, whose corresponding field is 16 bits. The FCS length is meaningful only when its presence flag is set. Reserved bits are checked and rejected when nonzero rather than being silently repurposed.

**Wire-format rule:** parse independently defined bitfields independently. A presence/validity bit gates interpretation of its associated value field.

**Compatibility rule:** preserve the standardized shared-width portion of an identifier when related file formats expose different container widths; do not allow extension bits to alter the base identifier.

**Reserved-bit rule:** reserved bits are not an undocumented extension namespace. Validate them according to the format contract until a standard assigns semantics.

## Propagate decoded metadata into packet processing

!6362 stores the FCS length in libpcap reader state and passes it to pcap_read_post_process for each record.

**Architecture rule:** metadata decoded at file-open time must be carried to the layer where packet semantics depend on it. Do not successfully parse metadata and then drop it before normal record processing.

**Confidence:** Extremely high. Both accepted master changes were authored by Guy Harris.
