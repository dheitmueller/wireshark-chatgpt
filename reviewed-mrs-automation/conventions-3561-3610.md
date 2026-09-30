# Durable conventions from !3561–!3610

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Optional dependency functions: test the real link contract

Merged master !3570 adds Kerberos PAC ticket-signature verification. Review first exposed ordinary signedness/const correctness problems, then Anders Broman's Windows build showed unresolved references for Kerberos encode/decode helpers that were not exported by the Windows library. The accepted implementation probes the functions with CMake and compiles the optional verification path only when the required symbols are available.

**Build rule:** do not infer external-library function availability from headers, platform family, or successful builds on another OS. Feature-test the exact functions that optional code needs, with a check that exercises the link contract, and compile-guard the optional path when supported dependency versions/platform packages legitimately omit them.

## Version-gated compiler flags still need real workflow coverage

Merged master !3571 reverts Windows CET/EHCONT hardening even though the flags were version-gated to MSVC releases that advertise them. The combination triggered internal compiler/linker failures during incremental builds.

**Platform rule:** a compiler version check proves only nominal option availability. Security/hardening flags and unusual linker modes still need validation in the project's normal full and incremental build workflows on the target platform.

## Typed-item mask checks are semantic prompts, not blind rewrite rules

Merged master !3576, authored by Martin Mathieson, adds warnings for suspiciously wide masks and odd hexadecimal digit counts. The checker comment explicitly notes that wider masks can be intentional when related flags are displayed as one aligned word. Merged !3591 then fixes a mixture of cosmetic mask notation and a real RTCP field-registration mismatch, while !3577 and !3568 provide concrete width/encoding follow-through.

**Checker rule:** classify mask-shape findings by what the checker can actually prove. A suspicious representation should prompt review of the field's wire width, mask semantics, and all call sites; do not mechanically shrink a mask or field solely to silence a warning.

## Name state by its semantics, not by an assumed actor

Guy Harris-authored merged master !3588 and !3590 consistently rename packet-block state from 'user', 'changed', or 'edited' to 'modified'. Guy's rationale is that a block can be changed by Lua, by prior users, or by other mechanisms; 'edited by the user' is not an invariant of the state.

**Naming rule:** API names, flags, and comments should describe the state or contract that code can actually guarantee. Avoid names that embed an assumed actor or mechanism when multiple paths can produce the same state.

## Historical layering evidence for wmem

Merged master !3602 removes wmem's dependency on wsutil so it can remain independently reusable. Guy Harris explicitly asks whether the intent is to enable lower layers such as Wiretap to use wmem, and João Valverde confirms that avoiding duplicated utility implementations is part of the goal. Later reviewed !3636 is the stronger/current architectural step, moving generic wmem infrastructure into the lower shared utility layer while retaining higher-level scope lifecycle contracts.

**Architecture lesson:** when an early refactor removes a dependency to break a cycle, treat it as a step toward a coherent lower-layer ownership model rather than freezing that intermediate directory/library arrangement as the final architecture.

## Public/private CMake visibility is an external-consumer contract

Merged master !3603 changed epan's GLib include/link usage from PUBLIC to PRIVATE. An external plugin developer later reported that the example plugin no longer compiled because linking epan no longer supplied the GLib include path; Gerald Combs points to later !3891 as the correction. Later reviewed !3945 provides stronger accepted evidence restoring the required public interface.

**Build lesson:** changing CMake visibility can be source-compatible inside the monorepo while breaking supported external plugins. Validate public target usage from an out-of-tree consumer before narrowing a dependency from PUBLIC to PRIVATE.

## Generated protocol sources remain derived artifacts

Merged !3582–!3584 and backports !3561–!3562 change ASN.1/configuration/template inputs together with regenerated dissector C.

**Generation rule:** make semantic changes in authoritative ASN.1/config/template inputs and regenerate. Backports that carry generated output should preserve the same source-of-truth relationship rather than editing generated C alone.

## Submission scope and branch hygiene

Closed !3598 contains Anders Broman's review that combining too much Thrift work and unrelated changes makes a merge request harder to review. Closed !3575 was resubmitted as merged !3587 after branch/CI problems; Anders documents amending/rebasing the existing topic-branch workflow, and the successful successor includes a focused sample pcap.

**Submission rule:** keep reviewable feature slices focused and use a dedicated topic branch. Amend/rebase that branch as review proceeds; avoid fork-default/master branches when they interfere with CI or MR lifecycle.
