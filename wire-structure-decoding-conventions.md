# Wireshark Wire-Structure Decoding Conventions

This file records durable rules for mapping packet bytes to protocol structures. Current upstream APIs and protocol specifications remain authoritative.

## Decode wire structures by explicit offsets and endianness, not C struct layout

A packet format is defined by its wire layout, not by the host compiler's C object layout. Casting packet bytes to a C structure, or relying on `#pragma pack` to make that cast appear to fit, imports host alignment, padding, and object-representation assumptions into untrusted packet parsing. The risk increases further when the structure contains variable-length data.

Merged master MR !11038 fixes SMB2 Query Info decoding after review pushed the implementation away from a packed C-structure overlay. Peter Stuge specifically recommended parsing the bytes instead of relying on compiler packing/alignment, and Pascal Quantin objected to retaining the packing pragma. The accepted implementation reads fields at explicit offsets with TVBuff helpers such as `tvb_get_letohl()` and adds fields using the protocol's specified byte order.

**Implementation rule:** express a wire structure as validated packet offsets, field widths, and protocol endianness. Use TVBuff accessors and subset boundaries rather than casting a packet pointer to a C struct. Do not use packing pragmas as a packet-decoding mechanism.

**Architecture rule:** keep the in-memory representation, if one is useful, separate from the decoder that reads the wire representation. Variable-length and optional protocol fields should be discovered from packet lengths/offsets, not from `sizeof` or a compiler-specific trailing layout.

**Review rule:** flag packet-pointer-to-struct casts, packed protocol structs, and `sizeof(struct)` used as proof of an on-wire header size. Check that each field's offset and endianness come from the specification and that truncation remains governed by TVBuff bounds.

**Confidence:** Very high. Merged master correctness work whose review explicitly rejected struct-packing dependence and converged on TVBuff-native field decoding.