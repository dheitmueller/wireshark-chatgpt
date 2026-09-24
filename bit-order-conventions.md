# Wireshark Bit-Order Conventions

This file records durable conventions for deriving bit masks, shifts, and field positions from protocol specifications. Current protocol specifications and upstream dissector APIs remain authoritative.

## Derive masks from the protocol's explicit bit-numbering convention

Do not assume that a specification diagram uses the RFC-style or conventional bit-numbering orientation merely because the drawing looks familiar. Some specifications number or transmit bits in the opposite visual direction, and a mask copied from the diagram under the wrong convention can decode every field consistently but incorrectly.

Merged MR !11574 fixes the Wi-SUN FAN Join Metrics IE after exactly this mistake. The Wi-SUN specification explicitly states that fields are depicted in transmission order from left to right and that bits are numbered starting at bit 0 on the leftmost, least-significant side. The original dissector treated the two high bits as the metric ID and the low bits as the length. The accepted fix changes the ID mask from `0xfc` to `0x3f`, the length mask from `0x03` to `0xc0`, and shifts the length by six, matching Wi-SUN's stated convention. Alexis La Goutte approved and merged the correction.

**Implementation rule:** before deriving masks or shifts from a bit diagram, find the specification's definition of bit numbering, transmission order, and significance. Translate that convention deliberately into the host integer/mask representation rather than assuming the diagram follows another standards family's layout.

**Review rule:** when a group of fields appears systematically swapped or mirrored, check the specification's bit-order prose before treating individual masks as isolated typos. Prefer citing the exact bit-order rule in the commit or code comment when the convention is unusual enough to surprise future maintainers.

**Testing rule:** include values that distinguish the competing interpretations. A test vector with only zeroes, all ones, or symmetric bit patterns can pass under both mask orientations and therefore does not validate the mapping.

**Confidence:** Very high. This is a merged master correctness fix with explicit specification text explaining the failure and maintainer approval.