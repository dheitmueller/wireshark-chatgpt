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

## Keep helper names and return contracts synchronized with what the helper actually does

A helper whose behavior has changed through refactoring should not retain a historical name or return value that describes work it no longer performs. Stale names encourage callers and reviewers to reason from the old abstraction, while an obsolete return value implies a decision point that no caller actually has.

Merged master MR !21638, authored and merged by Guy Harris, renames `read_record()` to `add_new_record_to_record_list()` because the record has already been read before the helper is called; the helper conditionally adds that record to the capture-file record list after read-filter processing. The same change converts the return type from `bool` to `void` because no caller any longer consumes the result.

**Implementation rule:** after changing a helper's responsibility, re-audit both its name and signature. Name it for the current semantic action, and remove return values that no longer communicate information callers use. Do not preserve misleading interface shape merely to minimize the textual size of a refactor.

**Confidence:** Extremely high. Merged master cleanup authored and merged by Guy Harris, with the semantic mismatch and obsolete result explicitly documented in the MR rationale.

## Match pointer-to-pointer out parameters with pointer-sized storage

A pointer-to-pointer out parameter is a real storage contract, not an invitation to cast the address of a smaller scalar to make the compiler accept the call. If an API writes a `void *` through a `void **`, the destination must actually be pointer-sized storage; receive the pointer value in the declared type and then convert it explicitly to the scalar representation expected by the caller.

Merged master MR !20029, authored and merged by John Thacker, fixes an SSH call to `wmem_map_lookup_extended()` that passed `(void **)&sender_channel` where `sender_channel` was a `uint32_t`. On platforms where `void *` is wider than 32 bits, the callee can overwrite adjacent storage; the bug was observed as a segfault with the SFTP sample capture. The accepted code receives the result into `void *sender_channel_p` and then uses `GPOINTER_TO_UINT()` to recover the integer. Stable backport !20032 carries the same correction.

**Implementation rule:** never satisfy an out-parameter type by casting the address of differently sized or differently represented storage. Use an object of the API's exact output type, then perform an explicit checked/project-standard conversion after the call.

**Confidence:** Extremely high. Concrete merged crash fix authored and merged by John Thacker and propagated to a stable branch.

## Put operation-wide bookkeeping in the primitive that owns the operation

When every normal caller of a low-level operation must perform the same state update, make that update part of the operation itself rather than requiring callers to remember a second bookkeeping step. This reduces duplicated call-site logic and makes the state invariant hold automatically. Callers with deliberately non-linear behavior should save and restore the bookkeeping state around that exceptional operation rather than weakening the normal contract.

Merged MR !10105 moves `wdh->bytes_dumped` accounting into `wtap_dump_file_write()` and removes the repeated manual increments from capture-file writers. File formats that write packets and later seek back to fill a header preserve the correct logical byte count by saving and restoring `bytes_dumped` around the header rewrite. John Thacker approved and merged the change after rebasing it.

**Implementation rule:** if a state change is an invariant consequence of a successful primitive operation, update that state inside the primitive. Handle exceptional operations such as in-place rewrites explicitly at their call sites by preserving/restoring the state needed to retain the primitive's simple normal contract.

**Confidence:** Very high. Broad merged Wiretap cleanup approved and merged by John Thacker, removing duplicated accounting from many writers.

## Express read-only pointer contracts with `const` instead of forcing callers to cast

If an API consumes caller-owned data without modifying it, declare that pointer parameter `const`. Do not make every caller cast away constness merely to satisfy an unnecessarily mutable signature; the type system should document and enforce the API's actual ownership/mutation contract. When a fixed string's size is part of a bounded wire representation, a static array plus `sizeof(array) - 1` can also make the bound compile-time visible and avoid narrowing warnings that arise from runtime-sized `strlen()` results.

Merged MR !10048, authored by Guy Harris, changes the data parameter of `wtap_buffer_append_epdu_tag()` to `const guint8 *` because the routine does not and should not modify the supplied bytes. It also changes fixed protocol names to static arrays and uses `sizeof ... - 1` so compilers can prove that the Exported-PDU tag length fits its 16-bit field. Closed !10049 proposed a convenience macro for the same warning, but Guy folded the corrected underlying API and call-site treatment into !10048; therefore the merged implementation is the authoritative precedent.

**Implementation rule:** make non-mutating byte/string inputs const-correct at the API boundary. For fixed compile-time objects feeding bounded length fields, prefer representations that preserve compile-time size information rather than hiding it behind a pointer and recovering length dynamically.

**Confidence:** Extremely high. Merged API correction authored by Guy Harris, with the competing closed proposal explicitly superseded by the accepted implementation.

## Keep dissector headers limited to genuine cross-file contracts

A header is an interface boundary. Internal analysis structures, implementation-only state, and helpers should not be placed there merely because a similar large dissector exposes analogous internals. Put only definitions that another compilation unit or subdissector genuinely needs in the header; keep the rest file-local in the `.c` implementation.

Merged master MR !15440 added the IBM i TRCCNN RDMA dissector. During substantive review, Martin Mathieson asked whether the analysis structures in `packet-irdma.h` were actually needed outside the dissector. The contributor explained that only the per-packet data passed to potential subdissectors was cross-file contract material; Martin recommended moving the rest into `packet-irdma.c`, and the contributor did so before merge.

**Implementation rule:** design dissector headers from actual consumers outward. Expose the minimal shared data required by subdissectors or other source files, and keep private flow-analysis/state structures and implementation details in the source file. Do not copy another dissector's header surface without verifying that the same sharing requirement exists.

**Confidence:** Very high. Direct maintainer review on a merged new dissector, with the requested encapsulation cleanup incorporated before merge.

## Name API variants for the capability they add, not for when they were introduced

Temporal suffixes such as `_new` age badly. They describe a moment in the API's history rather than a stable semantic difference, and become actively ambiguous as soon as another revision appears.

Merged master MR !13975 adds human-readable descriptions to heuristic dissector lists. During review, Jaap Keuter objected to an initial `*_new()`-style API name because “new” is a temporal designation that expires with the next API change, and asked for a suffix that says what was extended. The accepted interface uses a capability-bearing name (`..._with_description`) while retaining the existing registration routine as the simpler wrapper.

**Implementation rule:** when extending an API while preserving an older entry point, name the new variant after the additional semantic capability, argument, or behavior—such as `_with_description`, `_full`, or another project-appropriate descriptive suffix. Avoid `_new`, `_newer`, version-era names, or other labels whose meaning depends on repository history rather than the call contract.

**Review rule:** read related function names side by side and ask whether a caller unfamiliar with their chronology can tell why one should be chosen over another. If the distinction only makes sense after knowing which was added later, the names are underspecified.

**Confidence:** Very high. Direct maintainer naming review from Jaap Keuter on a merged API extension, with the requested semantic rename incorporated before merge.

## Derive wire-format lengths inside semantic helpers when the data itself determines the length

If a serialized field's length is mechanically determined by the object being serialized, callers should not have to supply both the object and a separately computed copy of its length. Put that derivation and wire-width enforcement in the semantic helper so callers cannot accidentally disagree with the data or silently wrap a narrow length field.

Merged master MR !11754, authored and approved by Guy Harris, adds `wtap_buffer_append_epdu_string()` for Exported-PDU string TLVs. It replaces call sites that manually supplied string lengths—some of which Coverity had already found to be wrong—and clamps the derived length to the 16-bit TLV limit rather than narrowing a larger value with a cast. The helper then delegates to the lower-level byte/tag primitive.

**API rule:** when data and length are not independent caller choices, accept the semantic object once and derive the redundant metadata centrally. At bounded serialization boundaries, explicitly enforce the representable range before passing the value to the lower-level encoder.

**Confidence:** Extremely high. Merged master API cleanup authored and approved by Guy Harris, with concrete incorrect manual lengths and the 16-bit boundary identified in the MR rationale.

## Do not publish an internal helper merely because a new feature needs it internally

A feature can require a specialized internal operation without making that operation a useful or stable external contract. Before exporting a new helper, ask whether external consumers have a natural reason to call it directly; if the public abstraction can express the capability through existing structures or callbacks, keep the implementation helper private.

Merged master MR !11753 adds per-exception expert information to the Thrift subdissector interface. During the design discussion the contributor considered exporting a new `dissect_thrift_t_exception()` routine, then deliberately kept the expert-aware struct helper internal because callers naturally describe Thrift results/exceptions through the member/union definition instead. The same discussion declined to generalize the new expert-info field to every Thrift type and declined to remove the older `reply_field_id` facility merely because the new mechanism reduced its importance; the compatibility cost was not justified by the simplification. The final rebased/squashed change was approved and merged by Anders Broman after fuzzing.

**API rule:** expose the smallest stable abstraction external consumers need, not every operation the implementation happens to contain. When evolving an extensible dissector API, separately evaluate whether a new capability warrants a public entry point, whether broad generalization has real users, and whether removing an older extension point is worth its source/ABI compatibility cost.

**Confidence:** High. Merged master extensibility change with the public-versus-private decision and compatibility tradeoffs explicitly reasoned through in the MR discussion and accepted in the final implementation.

Merged master MR !11722, authored by Guy Harris, also reinforces the naming side of this file: `slist_break_commas()` was renamed `process_enable_disable_list()` because splitting comma-separated strings was only an implementation mechanic; the helper's stable responsibility was applying an enable/disable list through a callback.