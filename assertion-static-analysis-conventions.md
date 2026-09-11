# Assertions and Static-Analysis Conventions

This file records durable Wireshark conventions for distinguishing malformed-input handling from programmer invariants, and for responding to static-analysis findings.

## Keep packet-triggerable malformed input out of `DISSECTOR_ASSERT`

`DISSECTOR_ASSERT` is for developer/programmer invariants: conditions that should be impossible if the implementation is correct. A malformed or deliberately adversarial capture must not be able to trigger such an assertion merely by supplying an unexpected value or violating a protocol constraint. Validate packet-controlled conditions normally and return, reject, or report malformed data as appropriate.

Merged MR !24174, authored and merged by Michael Mann, removed several assertions that could be reached with intentionally malformed packet data. Examples include replacing checksum-range assertions with an ordinary `false` return and making a hash helper tolerate missing packet-derived identity state rather than asserting. The MR description explicitly distinguishes these cases from “developer error.”

**Confidence:** High. This is a merged maintainer-authored hardening change and is consistent with Wireshark's requirement that malformed captures be handled as input, not as internal-programming failures.

## Do not distort established dissector idioms merely to silence a static-analysis false positive

Static-analysis findings are evidence to investigate, not a requirement to mechanically rewrite correct code. When a warning is a false positive and the proposed workaround would make a dissector diverge from well-established Wireshark idioms, prefer improving the analyzer model, annotation, or narrowly scoped suppression instead of proliferating non-idiomatic rewrites.

Merged MR !24208 is strong maintainer evidence. Michael Mann explained that the reverted QCDIAG changes had been made only for Coverity, while hundreds of dissectors correctly use `val_to_str[_ext]` fallback formatting. He argued that improving Coverity's understanding is preferable to playing “whack-a-mole” with individual dissectors and breaking consistent dissection style. The same discussion also reaffirms `wmem_strbuf_t` with `pinfo->pool` as the preferred packet-scope mechanism for dynamic string manipulation; that allocator/string guidance is already represented elsewhere in this notebook.

This does **not** mean ignoring legitimate analyzer findings. In the same batch, merged !24207 makes a straightforward cleanup for a real Coverity complaint where the surrounding call contract already guarantees a non-NULL context. The distinction is whether the warning exposes a real invariant/bug or merely misunderstands a valid established pattern.

**Confidence:** High for the review principle. Direct maintainer rationale on a merged MR, with an adjacent merged analyzer cleanup illustrating the complementary case.

## Treat compiler attributes as semantic contracts, not decorative annotations

Attributes that tell the compiler about allocation, aliasing, ownership, or other behavior must match the complete semantics of the returned object. In particular, `G_GNUC_MALLOC` / GCC `__attribute__((malloc))` is inappropriate for a constructor whose returned object contains pointers to pre-existing objects such as its `wmem_allocator_t`: the compiler attribute promises non-aliasing properties that such an object does not satisfy. Incorrect annotations can therefore create optimizer-visible semantic lies rather than merely inaccurate documentation.

Merged master MR !23728, authored and merged by John Thacker, removes `G_GNUC_MALLOC` from `wmem_array_new`, `wmem_list_new`, interval-tree constructors, and similar APIs that retain an allocator pointer, while leaving the annotation on functions that return newly allocated standalone memory/strings where the contract is valid. Accepted release backports !23730 and !23731 repeat the correction. The MR explicitly ties the decision to GCC's documented rule that `malloc`-like functions may not return storage containing pointers to existing objects.

**Implementation rule:** before adding or preserving a compiler/analyzer attribute, verify the semantic guarantees the tool is entitled to infer from it. If an object retains or aliases existing state, do not claim a stronger allocation/non-aliasing contract merely because the top-level object itself is newly allocated.

**Confidence:** High. Merged master maintainer-authored change with accepted stable backports and explicit compiler-contract rationale.

## Assert low-level programmer preconditions without silently narrowing a higher-level API contract

A low-level primitive may legitimately require a non-NULL object and use `DISSECTOR_ASSERT` to document that programmer invariant, while a higher-level wrapper may intentionally support a nullable argument for historical or semantic reasons. Tightening the primitive must not accidentally narrow the wrapper's established contract; guard the call at the wrapper boundary and assert only where the invariant truly applies.

Merged MRs !23427 and !23428, both authored and merged by John Thacker, illustrate the two sides of this rule. !23427 restores the established `proto_tree_add_bytes*` behavior for a NULL tvb by avoiding `tvb_get_ptr()` when no tvb is present. !23428 then adds a `DISSECTOR_ASSERT(tvb)` inside `tvb_get_ptr()` itself, matching other tvb APIs and replacing an otherwise uncontrolled NULL dereference with an explicit programmer-invariant failure.

**Implementation rule:** when strengthening an internal API's preconditions, audit its wrappers and callers for intentionally broader contracts. Preserve supported nullable/sentinel behavior at the appropriate layer, and place assertions only at the layer where violating the precondition is necessarily a programming error.

**Confidence:** Very high. Two adjacent merged maintainer-authored fixes explicitly separate the public/wrapper compatibility contract from the low-level primitive invariant.