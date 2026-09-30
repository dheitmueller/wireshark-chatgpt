# Wireshark Bit-Order Conventions

This file records durable conventions for deriving bit masks, shifts, and field positions from protocol specifications. Current protocol specifications and upstream dissector APIs remain authoritative.

## Derive masks from the protocol's explicit bit-numbering convention

Do not assume that a specification diagram uses the RFC-style or conventional bit-numbering orientation merely because the drawing looks familiar. Some specifications number or transmit bits in the opposite visual direction, and a mask copied from the diagram under the wrong convention can decode every field consistently but incorrectly.

Merged MR !11574 fixes the Wi-SUN FAN Join Metrics IE after exactly this mistake. The Wi-SUN specification explicitly states that fields are depicted in transmission order from left to right and that bits are numbered starting at bit 0 on the leftmost, least-significant side. The original dissector treated the two high bits as the metric ID and the low bits as the length. The accepted fix changes the ID mask from `0xfc` to `0x3f`, the length mask from `0x03` to `0xc0`, and shifts the length by six, matching Wi-SUN's stated convention. Alexis La Goutte approved and merged the correction.

**Implementation rule:** before deriving masks or shifts from a bit diagram, find the specification's definition of bit numbering, transmission order, and significance. Translate that convention deliberately into the host integer/mask representation rather than assuming the diagram follows another standards family's layout.

**Review rule:** when a group of fields appears systematically swapped or mirrored, check the specification's bit-order prose before treating individual masks as isolated typos. Prefer citing the exact bit-order rule in the commit or code comment when the convention is unusual enough to surprise future maintainers.

**Testing rule:** include values that distinguish the competing interpretations. A test vector with only zeroes, all ones, or symmetric bit patterns can pass under both mask orientations and therefore does not validate the mapping.

**Confidence:** Very high. This is a merged master correctness fix with explicit specification text explaining the failure and maintainer approval.
## Carry bit-order semantics through extraction, array conversion, and presentation

Merged master MR !4200 extends Wireshark's bit APIs so bit numbering can be explicitly big- or little-endian. The change propagates the encoding through `tvb_get_bits*`, `tvb_get_bits_array()`, proto-tree bit helpers, and formatted bit display. Existing callers are explicitly passed `ENC_BIG_ENDIAN` to preserve their prior behavior while USB HID uses `ENC_LITTLE_ENDIAN`.

Capture-based review then exposed a remaining hidden assumption: the FT_BYTES path still called `tvb_get_bits_array()` as though bit numbering were always big-endian, producing incorrect USB HID padding. The API was extended there as well and the reviewer retested the capture successfully.

**Implementation rule:** when bit order is semantically meaningful, propagate it through every helper layer that reads, converts, or formats the field. Fixing only the leaf caller can leave a hidden default in a lower-level path.

**Compatibility rule:** when adding an explicit encoding parameter to a widely used API, make existing callers state the old behavior rather than silently changing their interpretation.

**Testing rule:** use capture values that distinguish the competing bit-numbering interpretations, including unaligned fields crossing byte boundaries. Byte-aligned or symmetric patterns can miss exactly this class of bug.

**Confidence:** Very high. Merged framework change with targeted capture testing and review-driven correction before merge.
