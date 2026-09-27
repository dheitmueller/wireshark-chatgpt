# Durable conventions from Wireshark MRs !7261–!7310

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This file collects the reusable coding, architecture, testing, and submission lessons promoted from this review run. Merged MRs are primary implementation evidence; closed MRs are used only where maintainer review clarifies a durable rule.

## Tear down UAT-derived state through the UAT reset lifecycle

Merged !7310 hardens SOME/IP configuration tables that maintain hash tables and dynamically derived lookup state. The accepted change separates teardown into explicit `reset_someip_*_cb` callbacks, wires those callbacks into the UAT registrations, and has the post-update callbacks rebuild from a clean state. It also rejects direct recursive self-reference and makes validation errors identify the offending record.

**Implementation rule:** if UAT rows feed secondary caches, indexes, hash tables, or dynamically registered state, implement the UAT reset callback so those derived objects are destroyed independently of successful post-update reconstruction. A reset, reload, or rejected edit must not leave old derived state alive.

**Validation rule:** configuration schemas that permit references between rows should reject trivial recursion/self-reference before building runtime state, and diagnostics should identify the row or key that failed.

## Repair static-analysis findings at the semantic data-flow cause

Closed !7266 and merged !7267 form a useful negative/positive pair. The original code used `ref` on the `range == NULL` path before assigning the current array element. !7266 tried to initialize `ref` to NULL and guard the use; Roland Knall pointed out that the operation was required and the actual fix was to establish `ref` before either branch. Gerald Combs closed that MR, and !7267 moved the current-element/layer initialization to the top of the loop while separately validating the empty-container case.

**Implementation rule:** when an analyzer reports use of an uninitialized value, determine where that semantic value is required to become valid and make that data flow explicit. Do not add a null/default guard merely to suppress the report if that guard would skip required behavior.

**Review rule:** distinguish a defensive check that represents a valid absent state from a band-aid that changes semantics. Static-analysis cleanup should preserve the intended operation, not just make the warning disappear.

## Separate heuristic recognition from ordinary dissection and explicit dispatch

Merged !7305 splits DCP-ETSI into a stricter heuristic recognizer and an ordinary dissector registered for Decode As. Merged !7302 adds a cheap content signature before TP-Link Smart Home claims traffic on an unassigned port. Merged !7269 rejects STUN values reserved specifically to avoid multiplexing collisions. Merged !7292 then shows the complementary stateful path: after a real STUN packet identifies the conversation, the ordinary dissector can own the conversation and handle later message forms that are intentionally too weak for heuristic recognition.

**Architecture rule:** heuristic entry points should answer “is this mine?” from protocol evidence; ordinary dissectors should perform the actual decode and remain usable through explicit dispatch such as Decode As.

**State rule:** once strong recognition has established protocol ownership, conversation state may route later weakly distinguishable packets through the ordinary dissector without weakening the stateless heuristic for unrelated traffic.

**Recognition rule:** exploit standards-defined impossible/reserved ranges and cheap protocol signatures as negative/positive evidence before claiming ambiguous traffic.

## Default port bindings should follow authoritative assignments

Merged !7303 removes KNX/IP's hard-coded claims on ports that are merely common in deployments and defaults its automatic TCP/UDP range preference to IANA-assigned port 3671.

**Registration rule:** do not convert deployment convention into a global automatic binding when the codepoint is not authoritatively assigned to the protocol. Preserve additional deployment ports through a configurable range or Decode As path instead.

## Use the common checksum presentation/status API

Merged !7304 computes STUN's protocol-specific FINGERPRINT value but presents and verifies it with `proto_tree_add_checksum()`, including the generated status field and bad-checksum expert item.

**Implementation rule:** protocol-specific checksum calculation can remain local, but use Wireshark's common checksum helper for value/status/expert presentation whenever its verification model fits. This keeps diagnostics and filter behavior consistent with other dissectors.

## Materialize expensive model values once in hot comparators

Merged !7296 changes packet-list sorting so each side's expensive `columnString()` result is produced once and reused for lexical comparison and optional numeric parsing. Guy Harris's review also forced the MR to state precisely what “Improve” meant: speed and memory behavior.

**Performance rule:** in comparison/sort paths that run O(N log N) times, avoid repeatedly crossing formatting/model boundaries for the same operand. Cache the derived value once per comparator invocation and reuse it.

**Review rule:** performance-oriented submissions should say which property is being improved and why; a vague “Improve” title is not a substitute for the cost model.

## Keep UI capability/state with the owning model and avoid needless state transitions

Merged !7297 exposes “can resolve this column” through a PacketListModel header role, eliminating a separately propagated `capture_file *` from the header widget. Merged !7280 avoids calling `setSortingEnabled()` when sorting is already in the requested state because the setter itself can trigger sorting/model churn. Merged !7309 clears the resolved checkbox when the option becomes inapplicable.

**Architecture rule:** model-dependent capability should be derived by the model (or a model role/API) rather than duplicated as widget-owned state or parallel dependencies.

**State-transition rule:** do not call side-effectful setters merely to reassert the current value when the setter can rebuild, sort, emit, or invalidate model/view state.

**UI rule:** when an option becomes inapplicable, its visual checked/selected state should not misleadingly imply that the option is still effective.

## Make project tools location-independent and explicitly Python 3

Merged !7290 ports the enterprise-number generator from Perl to Python while making the output path an explicit argument instead of changing directory relative to the script's own location. Gerald Combs requested `#!/usr/bin/env python3` and executable mode; merged !7264 applies the same Python 3 shebang/executable convention across project scripts.

**Tooling rule:** repository tools should accept their input/output paths explicitly and should not depend on being launched from a particular current working directory or on relocating themselves to the repository root.

**Python rule:** executable project Python scripts should identify Python 3 explicitly with the project-standard shebang and carry executable mode when intended to be launched directly.

## New dissectors need integration evidence, source hygiene, and release-appropriate scope

Merged !7306 went through substantial first-contribution review. Alexis La Goutte requested conventional registration placement, proper registration prototypes, warning cleanup, a sample capture, release-note coverage, and a squashed submission. Gerald Combs stated the release-scope rule directly: new dissectors normally ship in major “.0” releases, while supported stable branches take bug fixes rather than new dissector features.

The later portability discussion around the merged code also reinforced that compiler-specific packed-layout tricks are not a substitute for decoding the protocol-defined wire representation, and that a green pipeline is evidence only for the platforms/jobs that actually ran.

**Submission rule:** a new dissector should arrive as an integrated Wireshark feature, not merely a compiling source file: follow source/registration conventions, clear project checks, provide representative capture evidence, update release notes, and present reviewable commit history.

**Release rule:** do not propose a new dissector as a stable-branch backport merely because it merged on master; stable branches are normally for fixes.

**Portability rule:** avoid compiler-extension-dependent wire layout when the protocol can be decoded from explicit offsets/sizes, and do not infer cross-platform validity solely from a green but incomplete CI matrix.
