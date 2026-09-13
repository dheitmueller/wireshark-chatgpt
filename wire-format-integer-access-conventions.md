# Wire-format integer access conventions

## Decode the serialized representation, not an assumed host scalar

When bytes come from a capture/file/kernel ABI, select accessors from the format's actual representation rather than from a convenient C scalar type. Do not read an odd-sized or byte-string field through a 16/32/64-bit native-order accessor merely because the bits of interest would fit in that type.

Merged MR !20927, authored and merged by John Thacker, is a concrete portability example. Linux nl80211 exposes 11-byte HE and 9-byte EHT PHY Capability elements in their original little-endian IE byte order, unlike the 2-byte HT and 4-byte VHT attributes that are transported as native-order integer values. Wireshark only needed bits from the first byte, so the fix changed the code from `nla_get_u32()` to `nla_get_u8()` instead of pretending the larger byte string was a host-order 32-bit integer. This fixed big-endian hosts while making the wire representation explicit.

Practical rule: if only one serialized byte is needed, read that byte. If the field really is a multi-byte integer, use an accessor whose width and byte order match the specification exactly.

## Keep generic byte-integer primitives in wsutil

Merged MR !20916, authored by Guy Harris and merged after Anders Broman approval, consolidated Wiretap's duplicate byte-to-integer helpers into `wsutil/pint.h`, added helpers for one-byte and non-power-of-two widths, and converted consumers to include that header directly. It also used the generalized helpers for EyeSDN's 40-bit timestamps.

Treat this as strong architectural guidance:

- Generic fixed-width wire/file integer loads and stores belong in the common `wsutil` primitive layer rather than being reimplemented privately in Wiretap or individual dissectors.
- Support the actual serialized width, including widths such as 24, 40, or 48 bits, rather than widening via ad-hoc byte manipulation at each call site.
- Consumers should include the header that declares the primitive they use instead of depending on a transitive umbrella include.

This reduces duplicated endian logic and gives portability/static-analysis work one canonical implementation surface.
