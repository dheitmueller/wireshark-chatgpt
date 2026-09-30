# Durable conventions from Wireshark MR review !3761-!3810

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Packet-lifetime allocation must survive nonlocal dissection aborts

Merged !3767 contains direct Evan Huus review rejecting a seemingly tidy conversion of short-lived packet data to NULL-scope allocation plus manual free. Packet dissection can abort in the middle of a helper, bypassing lexical cleanup; using `pinfo->pool` ensures the allocation is reclaimed with the packet even on that path. Merged !3765 and !3783 independently carry the broader migration from ambient packet scope to explicit packet context.

**Rule:** for data whose semantic lifetime is the current packet, prefer `pinfo->pool` even when the object is lexically temporary. Manual free is not an improvement if exception/nonlocal-abort paths can skip it. Thread packet context through helpers where practical.

## Keep supported build backends semantically equivalent

Guy Harris-authored merged !3795 and !3800 add Meson/Ninja support for GLib and then ensure the Meson path receives the same macOS deployment-target and SDK flags as the autotools path.

**Rule:** adding a second build backend is not complete when it merely compiles. Preserve target OS, SDK, compiler/linker, and deployment semantics across supported backends; compare the effective build environment explicitly.

## Probe exact provider identity and clean up only resources the bootstrap script owns

Guy-authored merged !3796 asks the actual question—whether Apple provides Python—by testing `/usr/bin/python3`, not whichever `python3` happens to appear first in PATH. Guy-authored !3797 similarly records whether the setup script installed Meson and only uninstalls it when that ownership marker exists.

**Rule:** when installation/removal policy depends on who supplied a tool, probe the exact provider identity and retain provenance. Never infer ownership merely from a matching executable name, and never uninstall a user/system dependency the setup script did not install.

## Integrate platform dependencies through the consumer's authoritative discovery mechanism

Guy-authored merged !3799 generates pkg-config metadata for macOS' system libffi because GLib's configure logic—both autotools and Meson—uses pkg-config to decide whether libffi exists.

**Rule:** when a dependency exists but its consumer cannot discover it, fix the discovery boundary the consumer actually uses. Avoid parallel ad-hoc variables that leave different build backends with different capability views.

## Checker severity should reflect semantic certainty

In merged !3791, Martin Mathieson and Pascal Quantin distinguish true field/mask-width contradictions from warnings that have legitimate exceptions or are primarily readability preferences. Merged !3802 then formalizes warning and error counts so CI fails only for findings considered definite bugs.

**Rule:** a source checker should make high-confidence contract violations fatal and keep heuristic/style findings visible but nonfatal. A green CI result does not mean warning-only output is irrelevant; it means the hard-error threshold was not crossed.

## Moving a public generic helper to a lower layer is an ABI-and-test change

Merged !3789 moves byte formatting from EPAN to wsutil, moves symbol exports between package manifests, renames an API whose old name no longer fits, updates consumers, and adds unit tests.

**Rule:** when lowering a reusable helper in the dependency graph, move the whole public contract: declaration, implementation, ABI/export metadata, call sites, semantic naming, and tests. Do not treat library relocation as source-file movement alone.

## Generated dissector changes must originate in generator inputs

Merged !3764 resynchronizes ASN.1 generated output, while merged !3765 explicitly changes ASN.1 templates/configuration, regenerates the dissectors, and builds the result as part of the allocator migration. !3771 independently follows the same pattern for ITS formatting.

**Rule:** make semantic changes in the authoritative ASN.1/template/conformance input and regenerate. A direct generated-C edit that is not reproducible from those inputs is transient drift.

## Preserve deployed legacy wire encodings during standards transitions when practical

Merged !3780 updates PCEP from draft segment-routing encoding to RFC 8664 while intentionally retaining decoding of the deprecated TLV and supplies a Cisco capture containing both forms.

**Rule:** when a published standard replaces a draft encoding that remains deployed, prefer decoding both generations when their distinction is well-defined and safe. Test the current and legacy forms with real or representative captures; standards compliance should not unnecessarily erase observability of deployed traffic.

## Prefer established parsing abstractions over local offset-advancing macros

In merged !3766, Alexis La Goutte rejects a new macro that wrapped `proto_tree_add_item()` plus offset advancement and points the contributor to `ptvcursor`; the final change adopts the cursor abstraction.

**Rule:** when Wireshark already has a parsing helper that couples tree insertion with cursor movement, use it rather than inventing a dissector-local macro with the same state-transition semantics.

## Keep hash input aligned with equality identity

Merged !3790 corrects QUIC CID hashing after the byte range omitted part of the identity. Review explicitly compares possible hash inputs with the fields used by equality.

**Rule:** a hash key may use fewer bits than equality only if equal values always hash equally and the selected bytes are deliberate. Never derive the hash extent from an in-memory struct layout unless the exact byte range is part of the key contract.

## Submission hygiene is part of the reviewable artifact

Merged !3810 includes direct Jaap Keuter review on local indentation, trailing-whitespace checks and the Wireshark submission hook, plus Stig Bjørlykke's request to squash fixup-only commits before merge.

**Rule:** use the project's pre-submit hooks/checkers and clean fixup-only history before merge. Review iterations can exist during development, but they should not become meaningless permanent history.
