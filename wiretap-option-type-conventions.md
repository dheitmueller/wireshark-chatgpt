# Wireshark Wiretap Option-Type Conventions

This file records durable conventions for representing capture-format options in Wiretap. Current upstream source and the governing capture-format specification remain authoritative.

## Preserve the option's specified primitive type, even when internal storage uses a different type

A capture-format option has an on-disk signedness and width that are part of its semantics. Do not choose the Wiretap option type merely because an existing internal field happens to use a wider or signed representation for implementation convenience or sentinel values.

Merged master MR !13464, authored and merged by Guy Harris, fixes pcapng `if_fcslen`: the pcapng option is an **unsigned one-byte** value, even though Wireshark's internal per-interface FCS length is a signed 32-bit value where `-1` means unknown. The accepted code retrieves the option as `guint8` and prints it unsigned; the internal representation remains free to use its wider sentinel-bearing type after conversion.

Merged master MR !13475, also authored by Guy Harris, adds first-class signed `INT8`, `INT32`, and `INT64` option types to the Wiretap block-option API because some real pcapng options are signed. The implementation carries the signed type through add/set/get/copy operations instead of reinterpreting the bits through an unsigned option API.

**Implementation rule:** model the serialized option using the format specification's actual primitive type. Convert explicitly at the boundary to any different internal representation required by Wireshark. Do not let an internal sentinel convention redefine the on-wire type, and do not use an unsigned option API for a signed format field merely because the bit width matches.

**Review rule:** for every newly supported block option, verify width, signedness, byte order, multiplicity/default semantics, and units against the authoritative format definition. Then trace that type through parser, in-memory option storage, copying, writer/debug output, and any conversion into higher-level interface state.

**Confidence:** Extremely high. Both master changes were authored and merged by Guy Harris, and !13464 explicitly contrasts the pcapng wire type with Wireshark's different internal representation.

## Apply interface timestamp offsets as signed semantic values, not opaque metadata

If a capture-format option changes the interpretation of packet timestamps, preserving the option in metadata is not sufficient; the reader must apply its specified semantics to the timestamp value exposed by Wiretap.

Merged master MR !13494, authored and merged by Guy Harris, adds pcapng `if_tsoffset` support. The option is a signed 64-bit count of seconds that is added to packet timestamps to obtain absolute timestamps, with absence meaning an offset of zero. The implementation parses the option with the signed option support added in !13475, stores it in per-interface state, and applies it after converting the raw pcapng timestamp into seconds/nanoseconds. Guy also supplied a test pcapng whose stored timestamps plus the interface offset reproduce the original capture's timestamps, and explicitly corrected the surrounding libpcap discussion to note that the offset is signed in the pcapng specification.

**Implementation rule:** distinguish descriptive metadata from interpretation-changing metadata. When an interface option participates in the mathematical meaning of packet records, normalize it into the record values at the Wiretap read boundary unless the API explicitly promises raw/unadjusted values instead.

**Testing rule:** construct a capture where the stored packet timestamp and metadata adjustment are individually nontrivial, and verify the resulting absolute timestamp against an independent expected value. Include negative values for signed offsets so an accidental unsigned path cannot pass unnoticed.

**Confidence:** Extremely high. Merged master implementation and validation artifact from Guy Harris, backed by the pcapng type/semantics discussion and the signed option API added immediately beforehand.