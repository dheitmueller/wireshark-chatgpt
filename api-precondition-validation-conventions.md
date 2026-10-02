# Wireshark API Precondition Validation Conventions

This file records durable conventions for where semantic operation preconditions should be enforced. Current upstream source remains authoritative.

## Enforce semantic preconditions at the shared operation boundary

When a shared operation is valid only for a semantic subset of otherwise valid identifiers, validate that subset in the shared operation before side effects begin. A frontend may perform earlier validation for better diagnostics, but it should not be the only place protecting the invariant.

Merged master MR !13242 fixes a crash reachable through `tshark -U`: an arbitrary registered protocol/tap name could reach PDU export even though only export-PDU taps are valid for that operation. During review John Thacker specifically suggested moving the suitability test into `exp_pdu_pre_open()` in `ui/tap_export_pdu.c`; the accepted implementation does so, checking `get_export_pdu_tap_list()` before registering the tap listener. Merged release backports !13261 and !13262 carry the same shared-boundary validation to maintained branches.

**Implementation rule:** enforce an invariant where it first becomes required by the common operation and where all callers converge. Perform the check before listener registration, allocation, state mutation, or dereference that assumes the invariant. Caller-specific validation can improve UX but must not substitute for the common guard.

**Review rule:** distinguish syntactic validity from semantic suitability. A name can identify a real protocol or tap and still be invalid for a narrower operation such as exported-PDU capture. Tests should include values that are valid in the broader registry but invalid for the requested operation, because those are precisely the cases a generic existence check misses.

**Testing rule:** exercise the shared failure path through each frontend that can reach it, and verify that invalid-but-well-formed inputs fail cleanly before any operation-specific side effect occurs.

**Confidence:** Very high. The placement was proposed explicitly by John Thacker during review of a merged master crash fix and then preserved in two merged stable-branch backports.

## Distinguish caller contract violations from impossible internal control flow

A shared dissector-facing API should diagnose invalid arguments supplied by its caller at the API boundary, while still retaining internal assertions for states that should be unreachable after those checks. These are different failure classes: the first is an actionable dissector-programming error, while the second protects the implementation against its own control-flow assumptions being broken later.

Merged MR !1472 adds an explicit empty-field-array check to `proto_item_add_bitmask_tree()` and reports it with `REPORT_DISSECTOR_BUG` rather than allowing the bad call to crash indirectly. Follow-up merged MR !1495 contains especially useful Jaap Keuter review: functions exposed for dissectors to call should validate their parameters and report a dissector bug when a cross-check fails; deeper duplicate checks may deliberately remain `g_assert_not_reached()` as defensive programming in case an earlier validation path is changed. The accepted implementation therefore converts caller-controlled invalid display-base/length cases into `REPORT_DISSECTOR_BUG` while preserving unreachable-state assertions where they still represent core implementation invariants.

**Implementation rule:** validate caller-controlled API preconditions before entering the implementation body that assumes them, and report invalid dissector usage through the dissector-bug mechanism. Do not mechanically replace every internal unreachable assertion with a recoverable caller error; keep defensive assertions for states that are impossible if the validated control flow remains correct.

**Review rule:** when changing an assertion, first identify who controls the value and which layer owns the invariant. Ask whether a buggy dissector can supply the value directly, or whether reaching the condition would instead mean that core API logic contradicted an earlier exhaustive check.

**Confidence:** Very high. Both changes merged; the boundary between caller validation and internal defensive assertions was stated explicitly by Jaap Keuter during review and reflected in the accepted implementation.

