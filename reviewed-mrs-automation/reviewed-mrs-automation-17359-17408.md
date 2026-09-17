# Automated MR review: !17359-!17408

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Direction: descending from newest toward older MRs. Before selecting this batch, the existing per-run ledgers in `reviewed-mrs-automation/` and `reviewed-mrs.md` were consulted. The previously reviewed !17571-!17620 batch remains counted. The fifty highest-numbered corpus MRs not represented by existing tracking are therefore the contiguous set !17408 through !17359.

## Exact reviewed set

!17408, !17407, !17406, !17405, !17404, !17403, !17402, !17401, !17400, !17399, !17398, !17397, !17396, !17395, !17394, !17393, !17392, !17391, !17390, !17389, !17388, !17387, !17386, !17385, !17384, !17383, !17382, !17381, !17380, !17379, !17378, !17377, !17376, !17375, !17374, !17373, !17372, !17371, !17370, !17369, !17368, !17367, !17366, !17365, !17364, !17363, !17362, !17361, !17360, !17359.

Count: 50.

## Review weighting and durable observations

Merged master changes were treated as the strongest implementation evidence; merged release-branch cherry-picks were treated mainly as corroboration, and closed/unmerged changes as lower-confidence negative/contextual evidence.

- **!17408 (merged, release-4.4; John Thacker)** fixes WSLua mask conversion by using the 64-bit conversion path directly instead of passing through a 32-bit helper. Useful corroboration for the existing arithmetic/type-width rule: do not narrow through an intermediate representation when the API/result is intended to preserve the wider domain. Because this is a backport, the corresponding master change is the stronger evidence.
- **!17400 (merged, master; John Thacker)** makes the operands of high-bit NBD constants unsigned (`1U << 31`) rather than relying on signed integer-literal behavior. This reinforces existing signed/unsigned and shift-safety guidance rather than requiring a new convention.
- **!17390 (merged, master)** addresses an undefined shift in ICMPv6 RPL arithmetic. Later review discussion challenged whether the algebraically transformed expression preserved the protocol calculation and explicitly went back to RFC 6550. This is useful evidence that UB-removal refactors in protocol arithmetic must preserve protocol semantics, and that specification validation is appropriate when the transformed expression is non-obvious. The existing arithmetic/specification guidance is sufficient; no new notebook rule was added in this run.
- **!17380 (closed/unmerged)** attempted to reduce Qt interface-scan log spam by waiting on the mutex. The author later closed it because the warning was no longer observed after moving to Qt 5.15. This is negative/contextual evidence only: avoid retaining behavioral workarounds for symptoms that disappear with the supported dependency/runtime unless an underlying correctness issue remains.
- **!17370 (merged, master)** updates Thrift internal documentation after an attempted code cleanup made the implementation more convoluted. The accepted result documents the deliberately different error/offset semantics rather than forcing superficial uniformity. Useful corroboration that documentation is preferable to abstraction churn when two paths have genuinely different contracts and making them syntactically uniform would reduce readability.
- **!17360 (merged, release-4.4)** is a Coverity-driven qnet6 bitmask/string-conversion fix. It rewrites the iteration so the loop index directly indexes the string table while the tested mask is explicitly `1 << j`, avoiding the prior coupling between mask value and shifted array index. As a backport, it is corroborative rather than primary evidence.
- **!17359 (merged, master; Pascal Quantin)** removes an extra protocol-column separator before invoking the embedded EPS NAS dissector. This is a small accepted example of respecting column ownership/fencing when nesting dissectors; it does not justify a broader new convention by itself.

The remainder of the batch was scanned for merge outcome, purpose, substantive discussion, and final change context. No additional durable rule rose above conventions already captured in the notebook; routine backports, generated/data updates, localized dissector fixes, and changes without substantive human review were deliberately not promoted into general rules.

## Notebook action

No convention file was changed in this run. The useful lessons reinforce existing notebook guidance on arithmetic/type-width safety, specification-backed protocol semantics, weighting backports below master changes, and avoiding unnecessary abstraction churn. This run ledger itself is the only notebook update.

## Continuation

Rebuild the reviewed set from all tracking before the next run. If no newer gaps have appeared, the next descending candidate after this batch is !17358.
