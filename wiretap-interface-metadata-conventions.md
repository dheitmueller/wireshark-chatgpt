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

## Put shared per-packet interface identity at the layer that owns the semantics

A capture format should only participate in interface-ID mapping when the format actually has interface identity to preserve. If multiple readers need the same name/channel-to-interface machinery, the reusable mechanism belongs in common Wiretap infrastructure rather than being copied into one format family and forced onto unrelated readers.

Merged master MR !20470 refactors SocketCAN-based readers so formats with interface or channel information can attach interface IDs to packets. During review, Guy Harris asked which formats besides candump actually contain packets from multiple interfaces and a per-packet indication of where each packet was received or sent; he explicitly noted that those are the formats that need the mapping code. He also suggested that the mechanism should eventually be lifted to `wiretap/wtap.c` and generalized for iptrace, candump, and other formats that can use it. Michael Mann then identified PEAK TRC bus IDs and Busmaster channels as examples of real interface-like identity.

**Implementation rule:** model interface identity from the capture format's actual semantics. Do not synthesize multi-interface machinery merely because a sibling reader uses it. When the same interface-name/channel mapping pattern appears across unrelated capture formats, prefer a Wiretap-level helper with format-specific extraction feeding it.

**Review rule:** ask two separate questions: (1) does this format contain meaningful interface/channel identity for each packet, and (2) is the mapping implementation generic enough that it belongs above the individual reader? Keeping those questions separate prevents both metadata loss and premature abstraction.

**Confidence:** Extremely high for the semantic/layering guidance because it comes from an explicit Guy Harris review discussion on a merged master MR.

## Carry already-resolved interface metadata across the capture process boundary

When the parent capture application has already resolved user-visible interface metadata, pass that metadata to the capture child instead of asking the child to rediscover it from an interface name. Parent and child can have different discovery capabilities or platform behavior, and rediscovery can produce inconsistent IDBs and filenames.

Merged master MR !15765, authored by John Thacker and merged by Anders Broman, changes Wireshark/tshark capture startup so an interface description is always passed to dumpcap when available, not only for extcaps. The MR notes a concrete Windows loopback case where the child cannot retrieve the same description through the Win32 API; before the fix, captures launched through Wireshark/tshark could therefore differ from direct dumpcap capture in IDB description and temporary filename.

**Implementation rule:** treat resolved capture metadata as part of the process-boundary contract. If the parent has authoritative interface name/description or other capture metadata needed in file output, transmit it explicitly to the child rather than relying on environment-dependent rediscovery.

**Review rule:** compare direct-child invocation with parent-spawned capture on interfaces that are difficult to enumerate or describe, including loopback and extcap-like paths. Metadata equivalence is part of capture correctness, not merely cosmetic UI behavior.

**Confidence:** Very high. Merged master capture fix authored by John Thacker with a concrete cross-platform inconsistency described in the MR.

## Preserve section scope when mapping pcapng interface IDs

An interface ID in a pcapng packet record is not globally unique across the whole file. Each Section Header Block starts a new section-specific interface namespace, so an interface reference is identified by the section together with the per-section interface ID.

Merged master MR !13559, authored and merged by John Thacker, fixes display and writing of captures containing multiple sections by carrying the section number into interface name/description lookup and storing an explicit mapping from `(SHB number, interface number)` to a Wiretap-global interface number. When dumping pcapng, Wireshark currently emits one output SHB rather than reproducing every input section, so the accepted change uses that mapping to rewrite packet interface numbers into the flattened output namespace. The MR also notes that merge handling needs the same per-SHB-to-global mapping.

**Implementation rule:** whenever a format restarts an identifier namespace at a structural boundary, include that boundary in the internal identity key. For pcapng interface references, do not key lookup or mapping by `interface_id` alone; preserve section identity until an explicit normalization step maps `(section, interface_id)` into a global or output-local identifier.

**Writer rule:** if output deliberately flattens multiple input namespaces into one, make the remapping explicit and deterministic. Do not carry input-local IDs through unchanged merely because the numeric values happen to fit the output representation.

**Review rule:** test captures with at least two sections whose interface numbering overlaps, including repeated interface ID zero with different IDBs. Verify both user-visible interface names/descriptions and rewritten packet interface IDs after save/export/merge.

**Confidence:** Very high. Merged master architecture/correctness fix authored and merged by John Thacker; the section-scoped mapping and single-SHB dump behavior are stated directly in the MR description.