# Wireshark Structured API Evolution Conventions

This file records durable conventions for evolving structured machine-facing interfaces such as sharkd JSON methods. Current upstream source remains authoritative.

## Preserve legacy fields when their shape cannot be extended compatibly

A positional array can become an accidental compatibility trap: inserting new elements or changing element meaning can break clients that depend on position and length. When the old representation cannot grow safely, keep it intact and introduce a parallel representation whose structure is explicitly extensible.

Merged MR !15405 adds sharkd sub-stream support to the `frame` and `follow` methods. The existing `fol` result is an array of string arrays and could not gain stream and optional sub-stream metadata cleanly without compatibility risk. The accepted API therefore preserves `fol`, treats it as deprecated, and adds a new `followers` array of objects with named fields. New information is intended to grow in the object-based form. The MR also updates/adds unit tests for the new argument and fields.

**Implementation rule:** do not mutate a legacy positional schema merely because the new data is conceptually related. Preserve the old field for existing clients, add an extensible object-based replacement, document the migration/deprecation direction, and add tests that cover both compatibility and the new semantics.

**Confidence:** Very high. Merged sharkd API change propagated to a stable branch with explicit compatibility rationale in the MR description.

## Add optional parameters with defaults that preserve existing behavior

New structured-API capability should not silently reinterpret requests produced by older clients. When a new parameter refines the units, mode, or interpretation of an existing parameter, make it optional and choose the old behavior as its default unless there is an intentional compatibility break.

Merged master MR !15434 adds an optional sharkd `interval_units` parameter for I/O graphs, accepting seconds, milliseconds, or microseconds. Existing requests omit the field, so the default remains milliseconds—the historical interpretation of `interval`. The same MR adds unit tests for the new parameter and aligns sharkd's graph-item limit with the Qt implementation.

**Implementation rule:** when extending a stable machine-facing request schema, make refinements optional when practical and define omission to mean the pre-existing behavior. Test both omitted/default behavior and each new accepted form.

**Confidence:** Very high. Merged master API extension approved and merged by John Thacker with explicit backward-compatibility design and tests.

## Make machine identifiers round-trip through producer and consumer APIs

When one structured API emits an identifier that another API expects callers to feed back, both sides must agree on representation as well as numeric value. A producer that serializes an ID differently from the token/parser consumed by a follow-up request creates a machine-facing compatibility bug even if both displays look understandable to a human.

Merged master MR !9919 fixes Sharkd RTP handling where `rtp-streams` and `rtp-analyse` emitted SSRC as an unsigned decimal number while analysis/download tokens parse the SSRC as a hexadecimal string using `ws_hexstrtou32()`. The accepted API emits values such as `"0xd2bd4e3e"` consistently and adds regression coverage that takes the RTP identity through stream listing, analysis, and download, including the no-data error path.

**API rule:** define a canonical external representation for identifiers that cross method boundaries and use it everywhere the value is emitted or consumed. Treat output intended for subsequent machine input as part of the protocol contract, not merely presentation.

**Testing rule:** exercise the round trip, not only each method independently. A listing/search response should be usable directly to construct the related follow/analyse/download request without representation-specific client guesswork.

**Confidence:** Very high. Merged master Sharkd API correction with end-to-end regression tests; the mismatch and expected token syntax are stated directly in the MR.

