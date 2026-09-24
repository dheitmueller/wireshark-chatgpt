# Wireshark Wire-Structure Decoding Conventions

This file records durable rules for mapping packet bytes to protocol structures. Current upstream APIs and protocol specifications remain authoritative.

## Decode wire structures by explicit offsets and endianness, not C struct layout

A packet format is defined by its wire layout, not by the host compiler's C object layout. Casting packet bytes to a C structure, or relying on `#pragma pack` to make that cast appear to fit, imports host alignment, padding, and object-representation assumptions into untrusted packet parsing. The risk increases further when the structure contains variable-length data.

Merged master MR !11038 fixes SMB2 Query Info decoding after review pushed the implementation away from a packed C-structure overlay. Peter Stuge specifically recommended parsing the bytes instead of relying on compiler packing/alignment, and Pascal Quantin objected to retaining the packing pragma. The accepted implementation reads fields at explicit offsets with TVBuff helpers such as `tvb_get_letohl()` and adds fields using the protocol's specified byte order.

**Implementation rule:** express a wire structure as validated packet offsets, field widths, and protocol endianness. Use TVBuff accessors and subset boundaries rather than casting a packet pointer to a C struct. Do not use packing pragmas as a packet-decoding mechanism.

**Architecture rule:** keep the in-memory representation, if one is useful, separate from the decoder that reads the wire representation. Variable-length and optional protocol fields should be discovered from packet lengths/offsets, not from `sizeof` or a compiler-specific trailing layout.

**Review rule:** flag packet-pointer-to-struct casts, packed protocol structs, and `sizeof(struct)` used as proof of an on-wire header size. Check that each field's offset and endianness come from the specification and that truncation remains governed by TVBuff bounds.

**Confidence:** Very high. Merged master correctness work whose review explicitly rejected struct-packing dependence and converged on TVBuff-native field decoding.

## Validate the bounded record before forming or using a typed view

When a capture-file reader works from a bounded page or record buffer, the fixed header must be proven to fit inside that boundary before code treats the current offset as a complete typed structure. A cast does not establish that the bytes exist. Putting the bounds check first also keeps future field accesses from accidentally escaping the validation.

Merged master MR !10692, authored and approved by Guy Harris, hardens the NetScaler wiretap reader by checking that the full fixed record structure remains in the current page before assigning the typed pointer, and by adding the same length proof to several record-type branches before accessing their fields. Guy explicitly described the accepted ordering as "test-before-casting," matching the pattern used elsewhere. Release-4.0 and release-3.6 backports !10693 and !10694 preserve the change.

**Implementation rule:** before creating or dereferencing a typed view into a bounded file/page buffer, verify that the entire fixed portion required by that view lies inside the current boundary. Keep the proof adjacent to and before the view where practical; later semantic length checks do not substitute for the initial storage-bounds proof.

This is a defensive rule for legacy/file-reader code that still uses typed views; it does not weaken the stronger dissector rule above to prefer explicit TVBuff offsets and endianness over native C structure overlays for packet decoding.

**Confidence:** Extremely high. Merged master bounds fix authored and approved by Guy Harris and carried to two stable branches.
