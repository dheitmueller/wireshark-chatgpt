# Wireshark Capture-Metadata Presence Conventions

This file records durable conventions for extending capture metadata while preserving compatibility with older producers. Current upstream source and capture-format specifications remain authoritative.

## Distinguish “not supplied” from a known false value

When capture metadata is extended with a boolean property, older producers that do not know about the property must not be silently interpreted as having supplied a false value. If the transport or pseudo-header lacks an independent presence mechanism, carry an explicit validity/known bit alongside the value bit.

Merged master MR !15344 extends Peekremote V0 metadata with 6 GHz band information. During review, Alexis La Goutte asked why both `6GHZ_BAND_VALID` and `IS_6GHZ` were needed. The contributor explained that the validity bit distinguishes devices that know and intentionally report the band from older sniffer firmware that never supplied the new field; without it, old producers would incorrectly make an unknown band look like a known 2.4/5 GHz result. The merged design retains both dimensions.

**Implementation rule:** for backward-compatible metadata evolution, model at least three semantic states when they are possible on the wire: unknown/not supplied, known false, and known true. Do not overload a zero-initialized value bit as both “false” and “producer does not support this field.”

**Review rule:** when adding a flag to a fixed capture pseudo-header or metadata record, test how captures from older producers decode. Ask whether the default all-zero representation means a real value or absence of knowledge. Add an explicit validity/capability/presence discriminator when those meanings differ.

**Confidence:** High. The distinction was raised and resolved explicitly during review of a merged master capture-format change, and the accepted representation preserves the presence bit.