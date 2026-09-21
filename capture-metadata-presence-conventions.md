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