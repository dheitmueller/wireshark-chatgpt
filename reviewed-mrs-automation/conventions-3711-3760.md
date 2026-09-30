# Durable conventions from Wireshark MRs !3711–!3760

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Typed-item checker coverage follows the real API contract

Merged !3758, authored by Martin Mathieson, extends `check_typed_item_calls.py` to the `proto_tree_add_bitmask*` family. The checker explicitly accounts for those APIs placing the hf index in a different argument position and validates that the registered type is one of the integer/boolean types accepted by the bitmask helpers. Guy Harris's merged !3755–!3757 BTATT fixes change aggregate fields from `FT_NONE` to `FT_UINT24`, and Martin points to !3758 as the automated coverage for the bug class.

**Rule:** checker support should be added across all semantically equivalent API variants, but with argument parsing and valid-type sets modeled for each API family rather than copied blindly.

## Generated ASN.1 C is derivative, not the maintenance surface

Merged !3724 performed a broad mechanical `wmem_packet_scope()` to `pinfo->pool` conversion. João Valverde notes that generated ASN.1 dissectors must be changed in their ASN.1 configuration/templates and regenerated, and recommends regenerating after each automated conversion to catch drift. Merged !3742 is the follow-up conversion of those authoritative sources and generated output.

**Rule:** for generated dissectors, make semantic or allocator changes in the generator inputs and regenerate. A direct generated-C edit is incomplete even when mechanically correct.

## Use packet-context allocation and do not regress to ambient packet scope

Merged !3724 and !3742 continue the explicit `pinfo->pool` migration. In closed !3740, Pascal Quantin rejects a large patch that reintroduced `wmem_packet_scope()` over a previous intentional conversion and requires the current explicit packet-context ownership to be retained.

**Rule:** when packet context is available, prefer `pinfo->pool` to ambient global packet-scope lookup. Review large rebases for accidental reversal of intentional allocator-lifetime refactors.

## CI should isolate platform failures from architecture failures

During merged !3749, Guy Harris asks for a macOS x86 pre-merge runner in addition to the new ARM runner. The purpose is diagnostic separation: a failure common to macOS should not be conflated with an ARM-specific compiler, dependency, emulation, or tool-binary failure.

**Rule:** when adding architecture coverage for an existing platform, retain or add a baseline architecture job when practical so CI can distinguish OS-family regressions from architecture-specific failures.

## Safety limits need a concrete threat/failure model

Merged !3734 raises profile ZIP per-file acceptance from 512 KiB to 256 MiB because real SOME/IP/Signal-PDU configuration files can be many megabytes. Roland Knall explains that the original limit was a runaway guard for deliberately broken ZIP files; Guy Harris asks what malformed file actually produces an infinite loop.

**Rule:** do not retain, remove, or enlarge arbitrary parser/import limits without identifying what failure they mitigate. If a limit is the only protection against malformed-input runaway behavior, removing it should be accompanied by explicit malformed-input tests and a structural termination guarantee.

## Capture metadata options require both the right block and real presence

Guy Harris's merged !3719 creates a `WTAP_BLOCK_PACKET` before text-import code attaches packet options, then releases the block after writing. Merged !3718 adds the packet-flags option only when direction data was actually supplied.

**Rule:** option APIs require the correct metadata container to exist, and optional metadata should be serialized only when its semantic value is known/present. Do not manufacture presence from a default-initialized value.

## Keep transitive link interfaces as narrow as possible

Closed !3713 proposed restoring Gcrypt as a PUBLIC dependency of wsutil to fix an sdjournal link failure. Gerald Combs points out that most targets linking wsutil do not need Gcrypt and recommends adding `${GCRYPT_LIBRARIES}` directly to `sdjournal_LIBS`. Merged !3716 implements that targeted fix.

**Rule:** fix missing link dependencies on the narrowest target that actually consumes the symbols. Promote a dependency to PUBLIC only when downstream consumers genuinely require it as part of the library's interface.

## Prefer deterministic dispatch when a strong binding exists

In merged !3712, Jaap Keuter questions a UDP heuristic for SHICP and recommends direct `dissector_add_uint("udp.port", SHICP_UDP_PORT, ...)`, explicitly calling it more efficient.

**Rule:** when a protocol has a reliable fixed port/table key and no strong reason to recognize it elsewhere, prefer direct table registration over a heuristic that must inspect unrelated traffic.

## Reviewability is part of MR scope

Closed !3740 was too large for GitLab's review UI. Pascal Quantin requires it to be split into smaller commits/MRs and the contributor subsequently moves the work into successor MRs.

**Rule:** a logically related change can still be too large to review safely. If tooling cannot present the diff or reviewers cannot reason about it effectively, split the submission into reviewable units with explicit dependency/order where needed.
