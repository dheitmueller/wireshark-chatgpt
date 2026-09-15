# Wireshark Wire-Layout Conventions

This file records durable rules for keeping C representation details separate from serialized or captured wire layouts. Current upstream source remains authoritative.

## Do not use `sizeof(struct)` as a serialized wire-size contract

The in-memory size of a C structure can include implementation-defined padding that is not present in the protocol or capture-file representation. This is especially easy to miss when a structure contains 64-bit members: ABI alignment can add unnamed tail padding even though the serialized format contains only the explicitly defined fields.

Merged master MR !19937, authored and merged by Guy Harris, fixes the Netflix pcapng custom TCP-info option after its size check used `sizeof(struct nflx_tcpinfo)`. The actual option data was not required to contain the unnamed padding that common 64-bit ABIs add to the structure. The accepted change defines the option's wire size explicitly from the serialized data rather than deriving it from the compiler's structure layout.

**Implementation rule:** define serialized lengths from the protocol/file-format specification or from explicit field sizes. Use `sizeof(struct)` only when the external format explicitly defines the serialized representation as that exact ABI layout, which normal network and capture formats generally do not.

**Review rule:** when a parser, writer, assertion, or bounds check compares external data length with a C structure size, inspect the structure for alignment and padding and verify that the comparison is against the wire contract rather than an in-memory representation accident.

**Confidence:** Extremely high. Merged master correctness fix authored and merged by Guy Harris, with the ABI-padding rationale explicitly documented in the MR.