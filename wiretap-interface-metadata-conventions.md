# Wireshark Wiretap Interface-Metadata Conventions

This file records durable conventions for preserving interface metadata while reading, mapping, and writing capture-file records. Current upstream source remains authoritative.

## Derive output encapsulation from the actual interface metadata

Capture writers that build an output-interface/channel mapping must carry forward the encapsulation associated with the source interface rather than substituting a convenient or common link type.

Merged master MR !20636 fixes BLF interface mapping while writing capture files. During review, Guy Harris explicitly challenged a hard-wired `WTAP_ENCAP_ETHERNET` and asked why the code did not use `mand_data->wtap_encap`; that discussion was resolved before the MR was merged. The MR's broader purpose was to restore correct channel/interface mapping after an earlier change had mapped all channels to channel zero and broken interface-name resolution.

**Implementation rule:** when a Wiretap writer creates or looks up output interfaces, treat the input IDB/record metadata as authoritative for properties such as `wtap_encap`. Do not infer Ethernet merely because it is common, and do not collapse distinct input interfaces/channels if the output format has enough identity to preserve them.

**Review rule:** for capture-format conversions, audit the complete identity tuple used by the writer's interface map: source interface/channel identifier, encapsulation, and any other format-specific metadata that distinguishes records. A mapping that is unique only under one encapsulation or channel value can silently merge logically distinct streams.

**Confidence:** Very high for the encapsulation rule because it was an explicit inline review correction from Guy Harris in a merged master MR. The broader identity-preservation wording follows directly from the bug and accepted mapping design.

## Do not advertise metadata precision that the in-memory representation cannot preserve

Capture-file metadata must describe the precision actually retained by Wiretap, not merely the precision claimed by the source file. If the in-memory record representation has already rounded or truncated a value, a writer must not reproduce finer-grained metadata that implies the lost information survived.

Merged master MR !20533, authored by John Thacker and approved/merged by Anders Broman, centralizes this rule for pcapng Interface Description Blocks. `nstime_t`, and therefore `wtap_rec` and `frame_data`, store timestamps only to nanosecond precision. The accepted change normalizes IDBs at the common dump boundary so output cannot continue to claim picosecond or finer resolution after the timestamps have already been reduced. It also consolidates timestamp-offset/resolution adjustments that had previously been scattered through individual read/write paths.

**Implementation rule:** normalize representation-limited metadata once at the common writer/dumper boundary. Prefer a single invariant enforced for every output path over format-specific omissions or read-side hacks that can leave some conversion path inconsistent.

**Review rule:** when adding support for higher-precision source metadata, trace both the metadata and the corresponding value through the complete in-memory pipeline. A parser can correctly recognize high precision while the writer is still wrong if an intermediate type cannot represent it.

**Confidence:** Very high. The limitation, incorrect pcapng export behavior, and centralization rationale are explicitly described in the merged master MR.

## Compare optional metadata using its specified default semantics

Absence of an optional capture-format field is not necessarily semantically distinct from an explicit value. When the file format specifies a default, interface comparison and merge logic must compare effective values rather than raw option presence.

Merged master MR !20500, authored and merged by John Thacker, fixes IDB comparison for `if_tsresol`: absence means the pcapng-defined default value 6 (microsecond resolution). Thus an absent option and an explicit value of 6 identify the same resolution, while absence and a non-6 value do not. This matters because libraries, including Wiretap, may remove a redundant explicit default.

**Implementation rule:** normalize optional metadata to its effective semantic value before identity/equality decisions. Do not let harmless serialization differences split one logical interface, and do not treat a missing option as a wildcard when its specified default differs from the other side.

**Confidence:** Very high. Merged master fix authored and merged by John Thacker with the default/equality cases stated explicitly in the MR description.
