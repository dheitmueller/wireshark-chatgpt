# Wireshark API Precondition Validation Conventions

This file records durable conventions for where semantic operation preconditions should be enforced. Current upstream source remains authoritative.

## Enforce semantic preconditions at the shared operation boundary

When a shared operation is valid only for a semantic subset of otherwise valid identifiers, validate that subset in the shared operation before side effects begin. A frontend may perform earlier validation for better diagnostics, but it should not be the only place protecting the invariant.

Merged master MR !13242 fixes a crash reachable through `tshark -U`: an arbitrary registered protocol/tap name could reach PDU export even though only export-PDU taps are valid for that operation. During review John Thacker specifically suggested moving the suitability test into `exp_pdu_pre_open()` in `ui/tap_export_pdu.c`; the accepted implementation does so, checking `get_export_pdu_tap_list()` before registering the tap listener. Merged release backports !13261 and !13262 carry the same shared-boundary validation to maintained branches.

**Implementation rule:** enforce an invariant where it first becomes required by the common operation and where all callers converge. Perform the check before listener registration, allocation, state mutation, or dereference that assumes the invariant. Caller-specific validation can improve UX but must not substitute for the common guard.

**Review rule:** distinguish syntactic validity from semantic suitability. A name can identify a real protocol or tap and still be invalid for a narrower operation such as exported-PDU capture. Tests should include values that are valid in the broader registry but invalid for the requested operation, because those are precisely the cases a generic existence check misses.

**Testing rule:** exercise the shared failure path through each frontend that can reach it, and verify that invalid-but-well-formed inputs fail cleanly before any operation-specific side effect occurs.

**Confidence:** Very high. The placement was proposed explicitly by John Thacker during review of a merged master crash fix and then preserved in two merged stable-branch backports.