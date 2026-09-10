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
