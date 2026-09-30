# Wireshark Capture-Metadata Presence Conventions

This file records durable conventions for extending capture metadata while preserving compatibility with older producers. Current upstream source and capture-format specifications remain authoritative.

## Distinguish “not supplied” from a known false value

When capture metadata is extended with a boolean property, older producers that do not know about the property must not be silently interpreted as having supplied a false value. If the transport or pseudo-header lacks an independent presence mechanism, carry an explicit validity/known bit alongside the value bit.

Merged master MR !15344 extends Peekremote V0 metadata with 6 GHz band information. During review, Alexis La Goutte asked why both `6GHZ_BAND_VALID` and `IS_6GHZ` were needed. The contributor explained that the validity bit distinguishes devices that know and intentionally report the band from older sniffer firmware that never supplied the new field; without it, old producers would incorrectly make an unknown band look like a known 2.4/5 GHz result. The merged design retains both dimensions.

**Implementation rule:** for backward-compatible metadata evolution, model at least three semantic states when they are possible on the wire: unknown/not supplied, known false, and known true. Do not overload a zero-initialized value bit as both “false” and “producer does not support this field.”

**Review rule:** when adding a flag to a fixed capture pseudo-header or metadata record, test how captures from older producers decode. Ask whether the default all-zero representation means a real value or absence of knowledge. Add an explicit validity/capability/presence discriminator when those meanings differ.

**Confidence:** High. The distinction was raised and resolved explicitly during review of a merged master capture-format change, and the accepted representation preserves the presence bit.

## Validate truncated pseudo-headers at each field boundary

Capture metadata can be truncated in the middle of a pseudo-header. A reader or normalization path should therefore validate the availability of each field it intends to read or transform, rather than requiring the entire nominal header to be present before doing any work or assuming that one successful header-level check makes every member safe.

Merged master MR !14367, authored and merged by Guy Harris, fixes SocketCAN CAN XL pseudo-header byte swapping so each field is handled only when all bytes for that specific field are available. The accepted implementation also replaces dependence on a native C structure layout with explicit offset and length constants. Release-4.2 backport !14368 carries the same fix.

**Implementation rule:** for fixed-format pseudo-headers and other capture metadata, gate each independent field access on `available_length >= field_offset + field_length`. If a prefix is present, it is acceptable to normalize the fields that are wholly available while leaving later truncated fields untouched. Do not dereference or byte-swap a field merely because some earlier part of the pseudo-header exists.

**Portability rule:** describe on-disk or capture-record layout with protocol-defined offsets and lengths when the layout itself is the contract. Avoid using native `sizeof(struct ...)`, member alignment, or compiler padding as the authority for wire/capture offsets unless the format explicitly is that native ABI.

**Review rule:** test truncation at boundaries inside the pseudo-header, not only packets that contain either zero metadata or the complete header. Include cuts immediately before and inside multi-byte fields, and exercise the opposite-endian path when byte swapping is involved.

**Confidence:** Extremely high. The master fix and its stable backport were authored and merged by Guy Harris, and the MR explicitly identifies both per-field availability and independence from C structure layout as the intended design.

## Populate optional record metadata in both sequential and random-access reads

Merged master MR !6792, authored by Guy Harris, adds a section number to `wtap_rec` with `WTAP_HAS_SECTION_NUMBER`. The numeric member has a default value, but the presence flag is set only when the reader actually knows the section. The pcapng implementation sets the metadata in both sequential and seek reads. Frame dissection tests the presence flag before exposing the field and converts the internal 0-based index to a 1-based display value.

**Implementation rule:** optional capture-record metadata needs a validity/presence contract independent of storage initialization. Populate it consistently through sequential and random-access read paths so interpretation does not depend on how the record was reached.

**Presentation rule:** keep intentional internal-versus-display numbering conversion at the presentation boundary.

**Confidence:** Extremely high. Merged master Wiretap/frame change authored by Guy Harris.


## Let typed packet-block option presence carry optional metadata presence

Merged !3708 moves packet drop count, packet ID, and interface queue from fixed `wtap_rec` packet-header members plus parallel `WTAP_HAS_*` flags into typed `WTAP_BLOCK_PACKET` options. Consumers query the option and only expose the field when the typed getter reports success.

**Architecture rule:** when metadata is semantically an optional packet-block property, prefer the typed block-option system over maintaining both a fixed record member and a separate presence bit. The existence of the option is the presence discriminator; its typed payload is the value.

**Review rule:** when moving metadata into a block/option representation, audit every producer and consumer together—reader, writer, text import/extcap, frame display, and any scripting/export surface—so no path continues to rely on the removed parallel state.

**Confidence:** Very high. Merged cross-layer Wiretap/EPAN/extcap/text-import conversion.


## Present zero is not the same as absent metadata

Merged master MR !3643 moves packet flags from fixed `wtap_rec` storage plus a parallel presence flag into typed `WTAP_BLOCK_PACKET` options. Guy Harris's immediately following merged master MR !3655 sharpens the semantic contract: iptrace, Sniffer, and Peek classic records always provide packet flags even when the flag value is zero. For those formats, zero means a known value such as “no packet errors,” not “metadata was not supplied,” so the accepted code always emits `OPT_PKT_FLAGS` instead of conditioning option creation on a nonzero payload.

**Implementation rule:** determine optional metadata presence from the format contract or typed option/getter result, not from whether its numeric value is nonzero. A present zero-valued option remains semantically present.

**Review rule:** when converting fixed metadata plus presence bits into typed options, explicitly test three cases where meaningful: absent, present with zero value, and present with nonzero value.

**Confidence:** Extremely high. The representation change is merged master work, and the zero-versus-absence correction was authored and merged by Guy Harris.
