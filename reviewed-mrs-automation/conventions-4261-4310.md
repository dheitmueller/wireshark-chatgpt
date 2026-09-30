# Durable conventions from Wireshark MRs 4261–4310

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Do not let malformed packet values reach C undefined behavior or internal assertions

Merged master MRs 4292 and 4295, both authored by John Thacker, harden H.264/H.265 Exp-Golomb parsing. Oversized encodings are hostile packet content, not proof of a programmer bug: Wireshark clamps the represented value, attaches malformed expert information, advances according to the encoded bit extent, and avoids undefined operations such as shifting a 32-bit value by 32. By contrast, an early `DISSECTOR_ASSERT_FIELD_TYPE` remains appropriate for the programmer-controlled field-registration invariant.

**Rule:** validate packet-controlled arithmetic before the C operation that would overflow, shift outside the type width, or otherwise enter undefined behavior. Report invalid wire data through normal malformed/error paths; reserve assertions for implementation invariants that malformed packets cannot legitimately control.

## Keep reassembly navigation semantics distinct from grouping identity

Closed MR 4290 proposed adding `tcp.reassembled_in` to the final frame so every member of a reassembly could be selected with one field. Pascal Quantin objected because the field's purpose is navigation to the frame where reassembly occurs; putting it on that frame makes it point to itself. Ronnie Sahlberg confirmed that the omission was intentional and suggested a separate integer reassembly identifier if grouping is desired.

**Rule:** do not overload a field with a second semantic merely to make a filter/UI workflow convenient. If users need an equivalence-class/group identifier and the existing field is a directional reference, add a distinct identity field with the appropriate type and contract.

This MR is closed, so the proposed code is not precedent; the direct reviewer explanation is retained as negative design guidance.

## Higher-layer defragmentation requires the bytes that define the lower-layer fragment

Merged master MR 4283, authored by John Thacker, prevents RPC record-fragment defragmentation when TCP has not supplied the complete RPC fragment and desegmentation cannot or will not obtain the missing bytes.

**Rule:** a preference enabling protocol-level defragmentation does not waive transport-level completeness requirements. Before inserting a nominal protocol fragment into reassembly, prove that all bytes of that fragment are available; otherwise diagnose the missing segment and keep the higher-layer reassembly state untouched.

## Error strings returned through owned-output parameters must follow the ownership contract

Merged master MR 4284 returns USBDump `err_info` using allocated GLib storage rather than a string literal. Guy Harris authored both maintained-branch backports, MRs 4285 and 4286.

**Rule:** when an API's `gchar **err_info` contract transfers a string the caller will free, every failure path must return compatible owned storage even if the message is constant. Do not mix static literals with caller-freed diagnostics.

## Static-analysis scripts are part of the build matrix

Guy Harris-authored merged MR 4273 excludes `capture-wpcap.c` from a Clang checker on hosts where that Windows-only translation unit is not compiled. The issue surfaced while merged MR 4272 was fixing a valid no-libpcap build.

**Rule:** repository source checkers that compile or semantically parse translation units must use the same target/platform eligibility assumptions as the build. A file that only forms a valid translation unit under another platform's headers/defines should not fail an unrelated host-side checker.

## Detect dependency and runtime semantics directly

Merged master MR 4275 probes the concrete Minizip struct member present in installed headers because distributions can provide minizip-ng compatibility code under the traditional minizip package identity. Merged MR 4263 goes one level further and runs a configure test for the C99 `snprintf`/ `vsnprintf` truncation-return semantics Wireshark actually requires.

**Rule:** test the API shape or behavior consumed by the code. Package name, dependency family, version, and symbol presence are weaker evidence when downstream substitutions or runtime semantic differences are possible. If a semantic contract is mandatory, fail configuration early with useful target/compiler diagnostics.

## Separate target OS from compiler/toolchain identity

Merged MRs 4268 and 4265 avoid applying MSVC-specific packaging/options merely because `WIN32` is true while building with MinGW. The same series replaces `strftime` extensions not reliably supported by the target runtime.

**Rule:** use target-OS predicates for OS properties and compiler/toolchain predicates for compiler/runtime-specific behavior. A Windows target can be produced by materially different toolchains.

## Normalize external build-selection strings before branching on them

Merged MR 4264 lowercases an environment-provided target-platform value, accepts only the supported canonical choices, derives the processor architecture from that validated state, and prints the resolved configuration.

**Rule:** normalize user/environment build inputs at the boundary, validate the closed set of supported values, and fail early for unknown values rather than silently mapping them through an `else` branch.

## Prefer explicit declarations when project tooling needs to inspect them

In merged MR 4310, Alexis La Goutte asked the contributor to replace macro-generated value-string declarations with explicit declarations because they are easier to maintain and easier for project scripts/checkers to understand. The contributor adopted the change.

**Rule:** code-generation macros are not automatically an improvement when they hide declarations from maintenance/checking tooling. Prefer explicit table/enum declarations where project checkers depend on recognizable source structure.

## Submission corroboration

Closed MR 4309 adds lower-weight but direct maintainer workflow evidence: the MR title and commit subject should accurately describe the actual change; unrelated heuristic and data-table work should be split; and a contributor should present a clean/squashed topic history rather than relying on merge-time squashing to make a tangled branch reviewable. Merged MR 4301 independently reinforces descriptive commit messages, whitespace checks, and squashing. These are corroboration of existing notebook submission rules rather than new policy.
