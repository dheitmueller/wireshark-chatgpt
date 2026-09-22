# Wireshark Unit Contract Conventions

This file records durable conventions for values whose meaning depends on an explicit physical or protocol unit. Current upstream source and API documentation remain authoritative.

## Keep units explicit across interface boundaries

A numerically plausible value can still be wrong when adjacent APIs express the same concept in different units. Do not rely on a variable name or shared integer type to carry unit semantics; identify the unit required by each interface and convert exactly once at the boundary.

Merged master MR !13003, authored and merged by Guy Harris, fixes FCS-length propagation across pcapng/libpcap-related paths where different interfaces express the length in bits, octets/bytes, or 16-bit units. Merged stable backports !13004, !13005, and !13008 carry the same correction across supported release branches. The series is unusually strong evidence because the bug was not an arithmetic-width problem: the values were valid integers, but their *units* differed between interfaces.

**Implementation rule:** for lengths, rates, timestamp scales, alignment quantities, and similar values, make the unit part of the local contract. Prefer names/types/comments that preserve that meaning, and perform explicit conversion when crossing into an API with a different unit rather than normalizing implicitly in several callers.

**Review rule:** when a value is forwarded through multiple layers, audit the unit at every handoff, especially when two APIs use the same C integer type. Check multiplication/division factors against the authoritative API or file-format specification rather than assuming similarly named fields have identical representation.

**Testing rule:** include values that make unit confusion visible. Zero and one can mask many conversion errors; exercise nontrivial values whenever the format or API permits them.

**Confidence:** Extremely high. The master fix and three merged stable backports were authored by Guy Harris and address a concrete cross-interface unit mismatch.
