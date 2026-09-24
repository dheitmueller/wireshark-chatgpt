# Platform API Portability Conventions

This file records durable portability rules for operating-system and third-party C APIs used by Wireshark. Current upstream platform support remains authoritative.

## Standards membership does not imply availability on every supported platform

A facility may appear in a current portability standard without being implemented by every operating system Wireshark supports. Compile platform-dependent functionality against the actual feature or macro that makes the API usable, not merely against a broad OS-family test.

Guy Harris's merged master change represented in !11664, with merged release-4.0 backport !11668, guards rawshark's virtual-memory-limit option on `RLIMIT_AS` itself in addition to excluding Windows. The motivating case was OpenBSD 7.3, which did not provide `RLIMIT_AS` despite that limit appearing in the then-current Single UNIX Specification. The same change also reports the concrete `setrlimit()` failure instead of a generic error.

**Implementation rule:** when a system constant, declaration, or facility can legitimately be absent, gate the code and user-visible option on the actual compile-time capability. Do not infer availability solely from `_WIN32` versus Unix, BSD-family membership, or standards provenance.

**Diagnostic rule:** when an optional system facility is present but a call fails, preserve/report the native error reason where practical; support for the symbol does not guarantee successful use at runtime.

**Confidence:** Extremely high. Guy Harris-authored merged master fix plus accepted stable backport motivated by a supported real platform.

## Do not assign between merely similar API structs unless their types are guaranteed identical

Two API structures can expose members with the same names and apparent representation without being the same C type on every supported library/platform configuration. Whole-structure assignment therefore creates an unnecessary ABI/type assumption when only individual scalar fields need to be copied.

Merged master MR !11665, authored by Guy Harris, fixes `radiotap-gen` by copying `tv_sec` and `tv_usec` individually from `struct timeval` into `pcap_pkthdr.ts` instead of assigning the complete `struct timeval`. The MR states the key portability point directly: the timestamp member in `pcap_pkthdr` may not have the same type as `struct timeval`. Merged release backport !11671 carries the same correction.

**Implementation rule:** when an external API specifies a structure with compatible-value members but does not guarantee exact type identity with a local/system structure, copy the required members explicitly. Do not use whole-struct assignment based on coincidental layout or typedef choices on one build.

**Review rule:** distinguish value compatibility from C type identity. In code that crosses libc, libpcap, OS, or SDK boundaries, audit aggregate assignments and casts for assumptions the API contract does not actually promise.

**Confidence:** Extremely high. Guy Harris-authored merged master correction with accepted release backport and explicit type-portability rationale.