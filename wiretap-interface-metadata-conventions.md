# Wireshark Wiretap Interface-Metadata Conventions

This file records durable conventions for preserving interface metadata while reading, mapping, and writing capture-file records. Current upstream source remains authoritative.

## Derive output encapsulation from the actual interface metadata

Capture writers that build an output-interface/channel mapping must carry forward the encapsulation associated with the source interface rather than substituting a convenient or common link type.

Merged master MR !20636 fixes BLF interface mapping while writing capture files. During review, Guy Harris explicitly challenged a hard-wired `WTAP_ENCAP_ETHERNET` and asked why the code did not use `mand_data->wtap_encap`; that discussion was resolved before the MR was merged. The MR's broader purpose was to restore correct channel/interface mapping after an earlier change had mapped all channels to channel zero and broken interface-name resolution.

**Implementation rule:** when a Wiretap writer creates or looks up output interfaces, treat the input IDB/record metadata as authoritative for properties such as `wtap_encap`. Do not infer Ethernet merely because it is common, and do not collapse distinct input interfaces/channels if the output format has enough identity to preserve them.

**Review rule:** for capture-format conversions, audit the complete identity tuple used by the writer's interface map: source interface/channel identifier, encapsulation, and any other format-specific metadata that distinguishes records. A mapping that is unique only under one encapsulation or channel value can silently merge logically distinct streams.

**Confidence:** Very high for the encapsulation rule because it was an explicit inline review correction from Guy Harris in a merged master MR. The broader identity-preservation wording follows directly from the bug and accepted mapping design.
