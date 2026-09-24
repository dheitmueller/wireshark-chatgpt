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

## Report only the platform facts that the system API can actually guarantee

System introspection APIs can expose a compatibility or emulation view rather than the physical host identity. Do not turn a reliable coarse fact into an unreliable specific claim merely because a structure contains an architecture field.

Merged master MR !11469, authored by Guy Harris, extends Windows bitness reporting for ARM64. Its rationale documents an important `GetNativeSystemInfo()` limitation: an x86/x64 application running under emulation on ARM64 can be given an x86/x64 view rather than the host's native instruction-set identity. The accepted code therefore uses the result to report the trustworthy property—32-bit versus 64-bit Windows—but deliberately avoids claiming that the host itself is x86-64 or ARM64 when that distinction cannot be established reliably.

**Implementation rule:** distinguish the semantic guarantee of an OS query from the apparent precision of its return structure. Under emulation, compatibility layers, containers, or virtualization, prefer a coarser statement that the API actually guarantees over a more specific but potentially false platform identity.

**Review rule:** for platform-detection code, ask whether the API reports the process view, compatibility view, kernel view, or physical host. If those can differ, document the limitation and constrain user-visible diagnostics to what can be known.

**Confidence:** Extremely high. Guy Harris-authored merged master change with the emulation limitation and resulting presentation choice documented directly in the source/MR rationale.

## Remove obsolete compatibility branches when the supported baseline already requires the newer contract

Legacy platform workarounds should not survive indefinitely after the rest of the code and supported build baseline already depend on the APIs or structure members they were intended to avoid. Keeping such dead compatibility paths makes platform code harder to audit and can obscure the real minimum contract.

Merged master MR !11468, authored by Guy Harris, removes an old MSVC 6 workaround around `VER_NT_WORKSTATION`/`OSVERSIONINFOEX`. The rationale is not merely that MSVC 6 is old: neighboring Wireshark code already required the same modern declarations and members and had been building successfully, so the conditional workaround no longer represented an actual supported configuration. The accepted change centralizes the workstation test in one helper using the supported API directly.

**Implementation rule:** before preserving a historical `#ifdef` workaround, compare it with the repository's current compiler/SDK support floor and nearby code. If supported builds already require the supposedly unavailable facility, remove the workaround and make the real baseline explicit rather than maintaining contradictory compatibility paths.

**Confidence:** Extremely high. Guy Harris-authored and merged cleanup with the obsolete-support rationale stated directly.

## Decode wire status values in the protocol's namespace, not the host errno namespace

A numeric status carried on the wire is protocol data even when its values resemble, derive from, or are named after `errno`. The local process's `errno` numbering and `strerror()`/`g_strerror()` text describe the build host's ABI, which need not match the protocol-defined numbering, the peer's operating system, or even another architecture running the same operating system.

Merged master MR !11227, authored and approved by Guy Harris, removes `g_strerror()` from the 9P2000.L dissector. 9P2000.L deliberately exposes Linux-style error numbers, but Guy documents that Linux itself has architecture-dependent errno assignments; the accepted dissector therefore uses a protocol-side value table rather than asking the host libc to interpret the wire value. Merged release-4.0 and release-3.6 backports !11229 and !11230 carry the same correction.

Merged master MR !11228, also authored and approved by Guy Harris, makes the distinction even clearer for GlusterFS: GlusterFS maps native system errno values into its own `GF_ERROR_CODE_*` protocol namespace specifically so clients and servers with different host errno assignments interoperate. Wireshark now decodes that protocol namespace directly, including value 0 as `Success`, rather than passing it through `g_strerror()`. Merged backports !11231 and !11232 carry the accepted behavior to stable branches.

**Implementation rule:** decode a wire status against the specification or protocol implementation that defines the wire namespace. Do not pass a received numeric value to the local host's `strerror()` family unless the protocol contract explicitly says the wire value is the receiver's native errno namespace and that assumption is actually portable for the supported peers.

**Review rule:** when a dissector uses a libc/OS symbolic decoder for an on-wire number, ask whether the numeric namespace is guaranteed identical on every sender, receiver, architecture, and supported Wireshark host. Similar names or values on the developer's machine are not sufficient evidence.

**Confidence:** Extremely high. Two independent merged master fixes authored and approved by Guy Harris, plus four accepted stable backports, with the cross-platform numeric-namespace problem documented explicitly.