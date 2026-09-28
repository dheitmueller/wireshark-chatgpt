# Pcap pseudo-header byte-order conventions

Merged Guy Harris MRs !6067, !6074 and !6093 form a coherent accepted sequence for pflog metadata.

Pflog UID and PID fields are stored in the writer host's native byte order. When a pcap file or section has the opposite byte order, wiretap normalizes those pseudo-header fields before the dissector sees them. The accepted post-processing path first bounds the record using the smaller of captured and reported lengths, then checks the pseudo-header's own declared length reaches the optional UID/PID fields, and only then swaps the fields.

**Wiretap rule:** file-byte-order normalization for host-endian pseudo-header metadata belongs in the capture-file reader rather than in protocol presentation code.

**Bounds rule:** validate both the outer record availability and the pseudo-header's internal length/version boundary before accessing optional fields.

**Confidence:** Extremely high. All three changes were merged and authored by Guy Harris.
