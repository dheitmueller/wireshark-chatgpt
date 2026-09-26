# Wireshark Generated-Code Conventions

This file records durable conventions for generated source and CI validation. Current upstream generators and CI configuration remain authoritative.

## Regenerate generated dissectors in CI and reject hand edits to generated output

Generated dissector source is an artifact of its authoritative input and generator. Review should happen against those inputs; allowing committed generated output to drift independently creates changes that disappear or conflict the next time regeneration occurs.

Merged MR !22694, authored by John Thacker and merged by Anders Broman, adds CORBA IDL dissectors to the CI regeneration checks. The MR states the intended invariant directly: like Wireshark's other generated dissectors, CI should regenerate them and verify that contributors have not changed the generated code directly.

**Implementation rule:** when a generated source family is committed to the tree and its generator is available in CI, regenerate it from the authoritative inputs and fail on a diff. Fix the generator or source specification rather than patching generated output by hand.

**Confidence:** Extremely high. Merged master CI change authored by John Thacker and approved/merged by Anders Broman, with the generated-source invariant explicitly stated.

### Keep the generator capable of reproducing accepted hand-modernizations

Merged master MR !21028 updated both the Skinny generator template/input and `tools/parse_xml2skinny_dissector.py` after the checked-in dissector had accumulated accepted C99 type, include-cleanup, and loop-variable changes. Its explicit success criterion was that regeneration once again produced the same `packet-skinny.[ch]` files already in the tree. Guy Harris approved the MR and, when Alexis La Goutte suggested CI coverage, recommended first exposing generated-dissector regeneration through named CMake targets so contributors and CI need to know the target rather than the generator's procedural steps.

**Implementation rule:** when an accepted change modernizes generated output, carry the equivalent change back into the authoritative template/generator promptly and verify byte-for-byte regeneration parity. Prefer a stable build-system target for regeneration so local contributor workflows and CI invoke the same authoritative procedure instead of duplicating generator command sequences.

**Confidence:** Extremely high. Merged master correction explicitly restoring generator/output parity, approved by Guy Harris, with Guy also providing the build-interface direction for future CI enforcement.

## Generate derivative metadata instead of maintaining parallel hand-written copies

When a dissector is already generated, metadata that can be derived reliably from the same authoritative input should be generated as part of that workflow rather than maintained in a separate hand-written file. Parallel manual copies create another source of drift and unnecessary intervention.

Merged master MRs !21852 and !21858 move SGP22 type/export information toward the generator output, and merged master MR !21860 removes a manually maintained PKIX implicit export configuration in favor of the generated export. During !21860 review, Michael Mann stated the architectural preference directly: if the dissectors are generated, he wants the least amount of manual intervention, and asked whether the corresponding explicit-PKIX exports could also be generated.

**Implementation rule:** if the generator has enough information to produce export tables, type metadata, or equivalent derivative configuration, teach the generator/build workflow to produce it rather than keeping a parallel hand-edited representation. Treat the authoritative specification plus generator as the source of truth.

**Confidence:** Very high. Multiple merged master generator cleanups, reinforced by explicit generated-code maintenance guidance from Michael Mann.

## Preserve command-line option semantics while changing generator defaults

Making a generator behavior the default is not the same as reversing the meaning of the option that previously enabled it. Existing build scripts and contributor workflows can continue to pass the old option; silently making that spelling mean the opposite behavior creates a compatibility trap even if the new default itself is desirable.

Merged MR !21811 enables ASN.1 constraint checking by default in the Wireshark generator workflow. In follow-up discussion, John Thacker objected to flipping the historical meaning of `-C`: his preferred compatibility model was for `-C` to remain an enable/no-op once constraints are the default, with a separate way to disable them if needed. Stig Bjørlykke explained that the intended end state was to remove the transitional option entirely once the remaining dissectors were fixed rather than preserve an inverted meaning.

**Implementation rule:** when promoting an optional generator feature to the default, keep existing enable-option semantics compatible during migration. Prefer an explicit inverse option for temporary opt-out, or remove the obsolete enable option once migration is complete; do not reuse the old spelling to mean the opposite operation. Also audit helper symbol visibility and generated-call surfaces when a formerly optional generation path becomes universal.

**Confidence:** High. The default-on constraint-checking change merged, and the compatibility concern comes from explicit post-merge John Thacker review with a stated migration direction from Stig Bjørlykke.

## Change the authoritative generator input and commit the regenerated artifact together

When generated dissector output is checked into the repository, a correct submission has two parts: change the authoritative ASN.1/template/generator input and include the resulting generated source in the same MR. Editing only the generated source is wrong because regeneration will discard the change; omitting the regenerated artifact is also wrong because reviewers and CI need the checked-in tree to reflect the authoritative inputs.

Closed MR !16228 attempted to add ITS VAM support by editing generated `packet-its.c` directly. Martin Mathieson pointed out that ITS is generated and directed the contributor to make the change under `epan/dissectors/asn1/its` instead. The corrected merged successor !16245 updates the generator-side input, and Martin explicitly asked that the regenerated `packet-its.c` be included in the MR even though the pipeline can regenerate and verify it. The contributor did so before merge.

**Implementation rule:** identify the source of truth before editing generated dissectors. Modify that source, run the supported regeneration path, and commit both the authoritative input change and the expected generated output when that output is version-controlled. CI regeneration is a verification mechanism, not a substitute for submitting the generated artifact.

**Review rule:** a diff touching generated source without its authoritative input is presumptively incomplete; likewise, an input-only change that should alter committed generated output should be checked for a missing regeneration.

**Confidence:** Very high. The incorrect direct-edit MR was closed, the maintainer supplied the correct workflow, and the replacement MR using that workflow was merged.

## Treat vendored upstream generator synchronization as a semantic port

A vendored generator can share ancestry with an upstream project while intentionally differing in API contracts and generated-code semantics. Synchronizing it therefore requires reviewing upstream commits against Wireshark's local contracts rather than mechanically importing every adjacent change.

Merged master MR !16095, authored and merged by John Thacker, imports upstream PIDL support for an `int64` IDL type but explicitly skips the following upstream Samba change that switched generated calls to `dissect_ndr_int64()`. Wireshark's NDR dissector intentionally uses `dissect_ndr_uint64()` while presenting the result through signed `FT_INT64` fields, so the apparently natural upstream follow-on would have changed local behavior incorrectly. The MR documents that divergence rather than hiding it inside the vendor refresh.

**Implementation rule:** treat updates from a vendored generator's upstream as semantic ports. Review each candidate change against Wireshark's local runtime APIs, field semantics, compatibility constraints, and generated output. Cherry-pick selectively when necessary, and document deliberately skipped or adapted upstream commits so a future sync does not reintroduce them blindly.

**Review rule:** provenance or proximity to upstream is evidence that a change is relevant, not evidence that it is correct for Wireshark. Pay particular attention to commits that alter generated call signatures, signedness, field types, ownership, or wire-decoding behavior.

**Confidence:** Extremely high. The accepted merged change was authored and merged by John Thacker and contains an explicit rationale for importing one upstream generator capability while intentionally rejecting the adjacent upstream semantic change.

## Keep shared generator inputs reviewable and synchronize generator changes separately

When Wireshark intentionally shares an IDL or generator with another project, preserve a clean semantic relationship to that upstream source. Avoid wholesale whitespace rewrites or unrelated formatting churn in shared IDL because it obscures the protocol changes reviewers need to inspect and makes later bidirectional ports unnecessarily difficult. When the generator itself must change, keep that engine change separable from the protocol/IDL change if another project needs to import it independently.

Merged MR !11597 is a detailed PIDL/DRSUAPI example. Stefan Metzmacher, an authoritative Samba/PIDL maintainer, objected when a tab-to-space rewrite made the Wireshark/Samba IDL diff difficult to review and asked that the IDL remain close enough to Samba's version that the semantic changes could be ported cleanly. He also clarified that this source belonged under Wireshark's PIDL-owned tree and regeneration workflow rather than the neighboring `idl2wrs` path. The contributor moved the inputs accordingly and cleaned up the IDL diff.

The same review found PIDL-side changes that Samba should receive independently. Metzmacher explicitly requested that the generator fix use the appropriate force/regeneration path and be made as a separate commit so he could import it into Samba's PIDL. The accepted series therefore treated generator maintenance, shared IDL synchronization, and generated dissector output as related but distinct review units.

**Implementation rule:** for generator-owned dissectors, first identify the correct generator-specific source tree and canonical regeneration workflow. Do not choose an input directory or tool merely because it produces superficially similar output.

**Review rule:** keep shared IDL/specification diffs semantically focused. If formatting churn hides the actual protocol change, restore a clean relationship to the upstream/shared source before review. Fix deterministic generator defects in the generator rather than repeatedly polishing its output.

**Submission rule:** separate generator-engine changes from protocol/schema changes when their upstream destinations or cherry-pick paths differ. This makes synchronization with projects such as Samba practical and lets each change be reviewed at the layer that owns it.

**Confidence:** Very high. The MR was merged after extensive review, and the direction came from Stefan Metzmacher in his area of direct Samba/PIDL authority, with Wireshark maintainers incorporating the requested restructuring.

## Make regeneration a cheap CI invariant at the authoritative source layer

When generated source is version-controlled, CI should not merely build the checked-in artifact. It should run the canonical generator over the authoritative IDL/conformance inputs and fail if regeneration changes tracked files. That turns source/output parity into a continuously enforced invariant and catches both direct edits to generated files and forgotten regeneration after source changes.

Merged master MR !11208, authored by John Thacker, extends Wireshark's existing generated-dissector checks to PIDL output. The check regenerates the relevant sources and treats a resulting repository diff as failure. In review, Alexis La Goutte asked whether this belonged in a faster Commit Check and whether the same model applied to ASN.1; John noted that ASN.1 was already covered and that the PIDL step added only about four seconds, making the fail-fast code-check stage appropriate. Adjacent merged MRs !11209 and !11203 demonstrate the other half of the invariant: when generated DCE/RPC output had already been corrected, the conformance/source inputs were changed to reproduce that accepted output rather than leaving source and artifact divergent. Merged !11210 then regenerates DFS through the canonical PIDL path.

Merged !11174 provides operational corroboration for scheduled generation: an upstream Asterix specification change required Wireshark's generator/tests to be adjusted before the next automatic regeneration, and the accepted MR fixed the resulting CI expectation rather than allowing the scheduled update to land broken output.

**CI rule:** expose a deterministic regeneration path, run it in the cheapest suitable pre-merge stage, and fail if tracked generated artifacts change. Prefer an early/fail-fast check when regeneration is inexpensive enough that contributors get the source-of-truth error before expensive downstream jobs.

**Source-of-truth rule:** when the generated result is correct but regeneration does not reproduce it, repair the IDL/conformance/template/generator input. Do not normalize the generated file by hand and leave the authoritative source stale.

**Automation rule:** if generated content is refreshed automatically from an external specification, land generator/schema compatibility changes and matching tests before the scheduled refresh that depends on them.

**Confidence:** Very high. The core CI invariant is merged master work authored by John Thacker, aligns with an already-established ASN.1 check, and is reinforced by several accepted source/output synchronization MRs in the same series.

## Repeated maintainer corrections reinforce ASN.1 source-of-truth discipline

Several merged April 2023 MRs provide unusually direct Guy Harris evidence for the generated-source rule. In !10330 (SPNEGO) and !10341 (ATN-ULCS), Guy pointed out that the changed `packet-*.c` files are generated and that the corresponding ASN.1 template/configuration source must be changed and the dissector regenerated. He then supplied merged corrective MRs !10353 and !10351. Likewise, in !10343 he identified a direct edit to generated `packet-ilp.c`; merged !10352 applies the same change to `epan/dissectors/asn1/ilp/ilp.cnf`. Merged !10320 independently fixes GDT/Z39.50 templates after an earlier change had touched generated output only.

**Review rule:** when a change lands in generated ASN.1 output, verify that the authoritative template/conformance input carries the same semantic change. A correct generated diff does not excuse a stale source-of-truth file.

**Maintenance rule:** if a generated-output-only fix has already merged, follow it promptly with a source-side correction so the next regeneration cannot silently revert the behavior.

**Confidence:** Extremely high. Multiple merged corrections, including three authored by Guy Harris in direct response to generated-output-only changes.

## MR 9092 and 9094 corroboration

Merged master MR !9092 changes `tools/ncp2222.py` and regenerates the NCP dissector include as part of the same parser-correctness update; release backports !9102 and !9103 preserve that relationship. Merged MR !9094 likewise changes the PKIX Qualified ASN.1 conformance input and regenerated C together. These independently reinforce the source-of-truth rule for generated dissectors.

## Keep Werror for project-owned code and scope generator warning exceptions narrowly

Generated parser/scanner code can trigger compiler-version-specific warnings that Wireshark cannot directly fix without changing or upgrading the generator. That does not justify disabling warnings-as-errors for handwritten project code that shares the same target or source file.

Merged master MR !8876 enables `-Werror` in the Clang CI path. Merged !8874 initially limits Werror source sets to nongenerated files where Flex output was noisy; !8875 and !8885 identify Lemon/Flex-specific warnings. Merged !8898 then provides generator-specific `DIAG_OFF_LEMON()/DIAG_ON_LEMON()` and Flex equivalents and arranges Lemon `%include`/`%code` boundaries so generated code sits inside the suppression region while handwritten code is checked normally.

**Build rule:** keep the project's own source warning-clean under Werror. When generated code has unavoidable warnings, isolate the exception to the generator-owned region or generated source rather than weakening the whole target. Restore diagnostics immediately after the generated region.

**Maintenance rule:** prefer named generator-specific diagnostic wrappers over scattered compiler pragmas so the supported warning exceptions remain centralized and auditable.

**Confidence:** Very high. A sequence of merged master CI/build changes converges on narrow generator-specific suppression while expanding Werror coverage.
