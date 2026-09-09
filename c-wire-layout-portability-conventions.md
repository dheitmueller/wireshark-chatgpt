# Wireshark C Wire-Layout and Portability Conventions

This file records durable C representation and compiler-portability conventions extracted from upstream Wireshark review and merged implementations. Current upstream source remains authoritative.

## Construct wire octet strings as bytes, not compiler-packed native structures

Do not make protocol correctness depend on a compiler-specific packed-structure layout merely to construct or overlay an on-wire octet string. Native structure layout introduces compiler/ABI portability requirements that are unnecessary when the protocol representation is defined in bytes.

MR !17579 introduced direct GCC-style `__attribute__((packed))` use in AMR code and subsequently broke Windows/MSVC builds. Merged MR !17583 removed that attribute because it was not supported by all compilers. A follow-up proposal, !17592, attempted to introduce a project-wide `WS_PACKED()` abstraction. Guy Harris questioned compiler coverage and, more importantly, observed that the relevant existing structure overlay was a candidate for an array instead. !17592 closed unmerged, so it is not accepted implementation evidence by itself.

The strongest evidence is merged MR !17600, `RF4CE: construct octet strings in arrays, not structures.`, authored and merged by Guy Harris. It replaces the packed/native-structure representation with explicit fixed-size byte arrays, pointer advancement, and `memcpy()` for the wire-format fields.

**Implementation rule:** for byte-defined wire representations, prefer `uint8_t` arrays plus explicit offsets/copies over packed C structures. Add compiler-specific layout attributes only when the native-structure ABI itself is genuinely required and all supported compilers have a deliberately reviewed implementation.

**Review rule:** when a patch proposes a compiler-specific attribute abstraction, first ask whether the underlying representation can be expressed portably without any layout attribute. Also consider every supported compiler/toolchain, not only GCC/Clang and MSVC.

**Confidence:** Extremely high. A portability regression was immediately corrected in merged code, Guy Harris explicitly recommended arrays in review, and his subsequent merged master implementation embodies the array-based design.

## Use standard flexible-array members for variable-sized trailing storage

When a C object has variable-sized trailing data, use the standard flexible-array-member form (`member[]`) instead of the historical one-element trailing-array idiom (`member[1]`). Compute allocation size from `sizeof(struct_type)` plus the required trailing storage.

Merged MR !17612 (`DOF: Use a flexible array`) replaces `uint8_t oid[1]` with `uint8_t oid[]` and adjusts allocation to use `sizeof(DOFObjectID_t)` plus the dynamic byte count. The stated motivation includes making the representation clearer to static analyzers.

**Implementation rule:** prefer standard C flexible arrays for variable-length trailing storage. Avoid fake one-element arrays that obscure the object's real bounds and can confuse static analysis.

**Confidence:** High. The reviewed MR is a merged stable-branch backport of an accepted master implementation.

## Do not cast byte buffers to wider integer pointers unless alignment is guaranteed by the representation contract

A byte buffer can be correctly sized yet insufficiently aligned for a wider native load. Casting a `uint8_t *` to `unsigned int *`, `uint32_t *`, or another wider pointer and dereferencing it is undefined behavior when the source allocation or offset does not guarantee the required alignment.

Merged MR !25451, authored by John Thacker, removes an XOR hash optimization in TLS that cast `StringInfo` byte data to `unsigned int *`. Clang UBSan reported a real misaligned load. The MR deliberately chooses a simpler alignment-safe hash because the path is not performance-critical enough to justify depending on special allocation alignment.

**Implementation rule:** treat arbitrary packet/string byte storage as byte-aligned unless its API explicitly promises stronger alignment. Use byte-wise operations, `memcpy()` into an aligned local, or an existing endian/load helper rather than dereferencing a wider cast pointer. Optimize only after preserving the representation's actual alignment contract.

**Confidence:** Extremely high. Merged master UB fix authored by John Thacker and demonstrated by UBSan with a concrete misaligned access.
