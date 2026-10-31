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

## Keep ASN.1 templates and conformance inputs synchronized with generated dissector fixes

A change that happens to fix `packet-*.c` may still be incomplete if that file is generated by `asn2wrs`. Inspect the corresponding `epan/dissectors/asn1/<protocol>/` template and `.cnf` source; make the semantic correction there and regenerate the checked-in output. A later correction to the generator input is not merely cleanup: it prevents the fix from being lost on regeneration and makes future backports reproducible.

Guy Harris's substantive review of merged !10330 (SPNEGO offset), !10341 (ATN-ULCS typed items), and !10343 (ILP string conversion) explicitly pointed out that the contributors had changed generated C rather than its authoritative ASN.1 input. Guy followed with merged template/conformance repairs !10351, !10352, !10353, !10356, and !10357. Merged !10320 and !10326 independently demonstrate the same template/output parity requirement.

**Review rule:** when the changed path is a generated dissector, verify that the same fix is represented in the source template, conformance file, ASN.1 definition, or generator, and that regeneration reproduces the committed C. Apply this check to tiny bug fixes as well as large regenerated changes.

**Confidence:** Extremely high — repeated direct Guy Harris feedback and his accepted corrective MRs; consistent with the existing generated-source CI invariant.


## Repair troublesome generator inputs rather than normalizing manual edits to generated dissectors

Closed MR !9930 proposed manual changes in generated DCE/RPC SRVSVC C/header files to repair `NetShareEnumAll`. Alexis La Goutte explicitly requested matching updates to `srvsvc.idl` and `.cnf`. The contributor found that the PIDL README documented known problematic input for SRVSVC and that regeneration had substantial side effects; Alexis recommended a separate MR rather than using the closed generated-only patch as the accepted implementation.

The follow-up **merged master MR !10009**, already present in the review corpus, updated the authoritative SRVSVC IDL/CNF alongside regenerated source, using `tools/pidl/pidl --ws-parser -- srvsvc.idl`. Alexis also requested fixing trailing whitespace and unused field-registration warnings, pointing to pre-commit hooks and `tools/check*` scripts. This is a concrete example of repairing the generator workflow and running source checks rather than accepting a divergent artifact.

**Review rule:** when generated-source regeneration is difficult or produces unexpectedly broad changes, verify the generator family's documented limitations and repair the canonical inputs/generator as a separate, reviewable step. Review both generated output parity and local source-check warnings. Do **not** cite the closed !9930 generated-only patch as an accepted implementation.

**Confidence:** Very high for the rule. Direct Alexis La Goutte review, closed initial proposal, and merged source-plus-output successor !10009; the specific 2023 PIDL limitations should be rechecked before assuming they persist today.

## Keep ASN.1 template and CNF changes aligned with generated dissector repairs

Correcting `packet-*.c` by itself can produce a short-lived fix if the definition lives in an ASN.1 `.cnf` or generator template. The next regeneration can silently restore the defect.

Merged master MR !9893, authored and merged by John Thacker, repairs six Coverity-reported NR-RRC field-length calculations where `(value=0)` accidentally assigned zero instead of comparing with `(value==0)`. Crucially, it fixes **both** `epan/dissectors/asn1/nr-rrc/nr-rrc.cnf` and generated `packet-nr-rrc.c`; release backports !9898 and !9899 preserve both. In separate merged master MR !9892, also by John Thacker, field-type/mask corrections initially made in generated ANSI MAP/TETRA dissectors are explicitly carried back into `packet-ansi_map-template.c` and `packet-tetra-template.c`, because otherwise regeneration would undo them.

**Implementation rule:** identify and patch the authoritative `.cnf`, template, grammar, or generator input whenever changing generated dissector behavior. Regenerate the output, diff-check parity, and add targeted cases for the original error. Treat “fix the template too” as a correctness requirement, not optional cleanup.

**Confidence:** Extremely high. Two independent merged master corrections authored and merged by John Thacker, plus matching stable-branch copies.

## Generated parser correctness fixes belong in the generator and emitted output

Merged master !9092 (Gerald Combs) adds checked arithmetic and an Expert Info field for malformed NCP offsets to both `tools/ncp2222.py` and generated `packet-ncp2222.inc`, with release backports !9102/!9103. In independent merged !9094 (John Thacker), a QCStatement must clear the global `object_identifier_id` at entry, because invalid BER may skip the callback that normally assigns it; the fix edits both `asn1/pkixqualified/pkixqualified.cnf` and generated `packet-pkixqualified.c`.

**Rule:** when a generated dissector needs a new defensive check, state reset, or Expert Info registration, patch the authoritative generator or ASN.1 configuration, regenerate the emitted code and verify synchronization; otherwise the bug returns on regeneration. Include malformed inputs that take the initialization-skipping/error path. **Confidence:** extremely high; separate merged fixes authored by Gerald and John, one with stable backports.

## Make generated ASN.1 source-line diagnostics opt-in without diverging from the generator

Merged **!8955** updates the `asn2wrs` CMake invocation to supply `-L` (suppress generated `#line` directives) by default while allowing debugging with `-DENABLE_DEBUG_A2W=ON`. Merged **!8959** regenerates the affected ASN.1 outputs. The generated-file churn is a derivative of one configuration decision, not independent protocol changes.

**Rule:** put generator behavior in the common CMake/generator entry point, regenerate affected outputs, and preserve a documented way to enable source mapping when debugging generated dissectors. Review generated-output churn against the changed flag rather than treating hundreds of emitted files as individual hand edits. Merged **!8942** illustrates a related hazard: an ASN.1 field-ID fix accidentally reverted an independent field fix in both `s1ap.cnf` and `packet-s1ap.c`; Pascal Quantin spotted it and merged **!8943** restored the previous behavior. Check *unrelated hunks* whenever regenerating. **Confidence:** very high from merged source/generator changes and direct Pascal review.

## Keep warning enforcement for handwritten code while bracketing generated parser bodies

Merged **!8876** enables `ENABLE_WERROR` in the Clang code-check job and GCC Release build; merged **!8874** narrows warning-as-error source lists to handwritten C files, excluding generated Flex/Lemon sources. Merged **!8898** introduces balanced `DIAG_OFF_LEMON()` / `DIAG_ON_LEMON()` guards around Lemon-emitted code and function-style Flex guards; preceding **!8875** and **!8885** show the piecemeal generator-warning failures this consolidation addressed. Later QCustomPlot documentation-warning repairs **!8879** and **!8882** likewise reflect compiler-version-sensitive third-party code.

**Rule:** retain strict diagnostics for project-owned code without applying brittle global suppressions to generator output. Place narrow, balanced guards at the generation boundary, verify their ordering in emitted files, and test the oldest supported generator/compiler plus platform-specific Clang documentation warnings. A merged warning-suppression patch is not evidence that generated code itself was semantically reviewed. **Confidence:** high, merged CI and generator changes.

## Migrate a generator before regenerating its output, and synchronize authoritative inputs

Merged **!8777** ports `parse_xml2skinny_dissector.py` to Python 3, with release backports **!8778** and **!8779**. The change deliberately defers regeneration so the toolchain transition can be reviewed/backported independently. Merged **!8780** and backport **!8781** then reconcile `packet-skinny.c.in` and `SkinnyProtocolOptimized.xml` with previously hand-modified generated `packet-skinny.c`, including a tabs-versus-spaces failure in Python-driven generation. Merged **!8782** updates both the generated dissector and its `.c.in` source when repairing displayLabel encoding.

**Rule:** do not repair only emitted source. Test the migrated generator, update its source template/schema, regenerate, and check for unrelated output changes. Separate a build-tool migration from semantic regenerated changes when that helps stable-branch backports; count related backports as corroboration, not independent design evidence. **Confidence:** very high for the accepted linked sequence.
## Track protocol-specification versions when correcting generated ASN.1 limits

Merged **!8698**, authored by **Guy Harris**, replaces an E2AP ASN.1 bound of 4294967295 with 1024 after Apple's Clang rejected conversion to the int parameter of dissect_per_constrained_set_of(). **Martin Mathieson** explained that the larger bound came from older O-RAN E2AP v2.01 material, whereas v2.03 changed it; his merged **!8700** documents that provenance next to the constant. The generated dissector value is updated alongside the ASN.1 source. Nearby merged **!8690** substantially upgrades E2AP and E2 service-model ASN.1 inputs, templates and generator behavior; its discussion also stresses not reading beyond TVB bounds when parsing shortString.

**Rule:** a narrowing diagnostic in a generator input may reveal a real change in the specification rather than merely a C type problem. Reconcile the cited spec version, preserve rationale for newer constants, regenerate related output consistently, and ensure decoder bounds checks remain valid. **Confidence:** very high for Guy's accepted correction and Martin's explicit source explanation.
