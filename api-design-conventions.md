# Wireshark API Design Conventions

This file records durable API and internal-interface design conventions extracted from upstream Wireshark merge-request review. Current upstream source remains authoritative.

## Name APIs for their semantic operation, not their current implementation mechanism

An interface name should describe what callers are asking Wireshark to do, rather than expose the library or system call currently used to implement it. This keeps implementation details from becoming part of the conceptual API and makes later implementation changes less disruptive.

Merged MR !24345, authored and merged by Guy Harris, renames interface-enumeration routines whose names were derived from `pcap_findalldevs()` to semantic names such as `get_local_interface_list()` and `get_remote_interface_list_common()`. The callers care about obtaining local or remote interfaces, not which libpcap entry point happens to provide them.

**Implementation rule:** choose function names from the stable caller-visible responsibility. Avoid embedding a specific backend/API mechanism in an internal interface name unless that mechanism is itself part of the contract.

**Confidence:** Very high. Merged architectural/API cleanup authored and merged by Guy Harris.

## Remove parameters and branches that no longer represent real caller choices

Do not preserve a boolean/mode parameter merely because an older implementation once had multiple paths. If every caller supplies the same value and the other path is dead or meaningless, eliminate the parameter and simplify the helper. File-private implementation helpers should also remain `static` rather than accidentally widening linkage.

Merged MR !24342, authored and merged by Guy Harris, simplifies interface-listing support after observing that all callers selected the same mode and the alternate branch could only return an empty result. It removes the constant boolean parameter and makes local helpers file-private where appropriate.

**Implementation rule:** periodically re-evaluate helper signatures after refactoring. A parameter is useful only when callers have a meaningful supported choice; dead configurability obscures invariants and makes maintenance harder.

**Confidence:** Very high. Merged cleanup authored and merged by Guy Harris, with the simplification directly reflected in the accepted code.

## Use the project's export mechanism as the source of truth for public dissector APIs

When a dissector header exposes an API, do not duplicate that public/private decision in ad-hoc header lists if Wireshark's existing export annotations and tooling already derive the public surface. Keep the public header's dependencies as narrow as the declarations actually require rather than pulling in broad implementation headers.

In merged MR !23934, Michael Mann adjusted a contribution that exported the DNS tap structure. He explicitly noted that `WS_DLL_PUBLIC` is enough for Wireshark's scripts to identify the header as public, so the header did not need to be manually added to a separate public-header list; he also removed an unnecessary `packet.h` include from the dissector header.

Merged MR !23121, authored by Jaap Keuter and approved/merged by John Thacker, provides complementary evidence for common headers: `epan/packet.h` stopped including all of `wiretap/wtap_opttypes.h` when most consumers only needed the opaque Wireshark-owned `wtap_block_t` handle. It uses an incomplete `wtap_block` type at that boundary, leaving sources that actually need the structure definition to include the defining Wiretap header themselves. This does not override the separate rule for dependency-owned public typedefs: external-library types must come from the dependency's authoritative header rather than being locally reconstructed.

**Implementation rule:** mark exported declarations through the established Wireshark export annotation/tooling path and avoid redundant public-header bookkeeping. Public and widely included headers should carry only the type definitions and includes required by their declarations; for Wireshark-owned opaque handles, an incomplete type can avoid forcing unrelated implementation dependencies on every consumer. For external-library types, use the authoritative dependency header instead of recreating its typedef.

**Confidence:** Very high. Direct maintainer correction by Michael Mann on a merged public-API change, independently reinforced by a merged header-dependency cleanup approved by John Thacker.

## Public C headers that support C++ must avoid C++-reserved identifiers

If a public C header is deliberately usable from C++ (for example, it contains `#ifdef __cplusplus` compatibility machinery), its exported identifiers must also be valid C++ identifiers. A name that is legal C but reserved by C++ can make an otherwise compatible public header uncompilable for downstream C++ consumers.

Merged MR !23452, authored by John Thacker, fixes `wiretap/pcapng_module.h` after a callback member named `new` caused C++ compilation failures. The accepted change renames the member and updates its call sites; Guy Harris additionally reviewed the replacement name for semantic clarity.

**Implementation rule:** review public/header-visible names against every language the header claims to support, not only the language used to build Wireshark itself. In C headers intended for C++ inclusion, avoid C++ keywords and other C++-reserved identifiers in fields, callbacks, parameters, macros, and exported declarations.

**Confidence:** Very high. Merged public-header compatibility fix authored by John Thacker, with direct Guy Harris naming review.

## Encode subsystem API restrictions in automated checks when possible

If a low-level library API is technically available but using it directly would violate a Wireshark architectural or security policy, do not rely only on reviewer memory. Put the restriction into the project's static API-checking machinery so new call sites fail review or CI close to the point where they are introduced.

Merged master MR !21953, authored by Gerald Combs and approved/merged by John Thacker, adds `gnutls_init` to `tools/checkAPIs.pl` with the explicit policy that Wireshark configures and uses GnuTLS session APIs for dissection only. The accepted change converts that architectural usage rule into an automatically enforced prohibited-API rule.

**Implementation rule:** when Wireshark has a project-wide wrapper, ownership boundary, or intentionally restricted use of a third-party API, prefer enforcing the forbidden raw entry points through `checkAPIs.pl` or equivalent tooling. Keep the nearby comment specific enough to explain the policy, not merely that the function is forbidden.

**Confidence:** Very high. Merged master policy/tooling change authored by Gerald Combs and approved/merged by John Thacker.

## Route callers through the stable semantic facade and keep presentation mechanics private

Once a subsystem has a stable caller-facing API for an operation, callers should use that semantic facade rather than invoking the lower-level mechanism that the facade currently delegates to. Besides making behavior consistent, this keeps the implementation behind the facade free to change without forcing callers to understand or preserve internal presentation policy.

Merged master MR !21861, authored and merged by Guy Harris, changes error and warning call sites to use the `report_*` routines rather than directly calling `failure_message` and `alert_box`, then makes those lower-level routines `static`. Guy's stated rationale is that reporting becomes consistent and the internal behavior of `report_*` and the routines beneath it can change as long as the external behavior of the `report_*` interface remains compatible.

**Implementation rule:** when a subsystem provides a semantic wrapper/facade, migrate callers to it and narrow the underlying implementation helpers to file-private or otherwise internal scope. Do not let convenience calls to a current UI/backend mechanism accidentally become a parallel API contract.

**Confidence:** Extremely high. Merged master API cleanup authored and merged by Guy Harris with the encapsulation rationale stated explicitly.