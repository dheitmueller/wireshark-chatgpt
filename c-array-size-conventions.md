# Wireshark C Array-Size Conventions

This file records durable C array-bound and array-size conventions extracted from upstream Wireshark review. Current upstream source remains authoritative.

## Array-size macros require an actual array, not a decayed pointer

Macros such as `G_N_ELEMENTS()` derive a count from the compile-time type of their argument. Once an array has decayed to a pointer—for example when it is passed as a normal function parameter—the macro no longer knows the original element count and must not be used as though it does.

Closed MR !21898 proposed fixing a Coverity array-bound warning by applying `G_N_ELEMENTS(prefix)` where `prefix` was a `const char * const *` parameter. John Thacker explicitly rejected that form: because `prefix` is a pointer at that point, `G_N_ELEMENTS` does not measure the original static array and in this case effectively yields the wrong bound. The unmerged MR is therefore useful as negative review evidence, not as an implementation exemplar.

Merged successor MR !21901 provides the accepted design. The SI and IEC prefix arrays remain visible where `G_N_ELEMENTS(si_prefixes)` / `G_N_ELEMENTS(iec_prefixes)` can be evaluated correctly, and the resulting count is stored in a `prefix_parameters` structure alongside the pointer, scale, and offset. Consumers then use `prefix_count` rather than trying to recover the array extent from a pointer. The change also adds tests at the upper limits that motivated the static-analysis concern, and John Thacker approved it.

**Implementation rule:** use `G_N_ELEMENTS()` only where the expression still has a true array type whose complete extent is known. If a helper needs both an array pointer and its bounds after decay, pass/store the count explicitly (or use a structure/API that preserves that metadata). Never infer a fixed-array count from a pointer parameter with an array-size macro.

**Review rule:** when static analysis reports a possible array overrun, check not only the comparison but also whether the expression used to derive the bound still denotes the backing array. A syntactically plausible bounds check with a pointer-derived `G_N_ELEMENTS()` can be worse than no fix because it appears self-maintaining while enforcing the wrong limit.

**Confidence:** Very high for the rule. It combines explicit John Thacker review rejecting the incorrect form with the merged, tested successor !21901. The abandoned !21898 implementation itself is deliberately down-weighted.

## Enforce fixed-array capacity at the consumer boundary as well as at producers

A structure that carries both a count and a fixed-size backing array has an internal invariant: the count must never exceed the array capacity. It is useful for in-tree producers to enforce that invariant before handing the structure off, but a consumer that indexes the fixed array should still validate the count it receives rather than relying on every current and future producer to have done so correctly.

Merged MR !26541 adds exactly those checks to the UMTS FP dissector for `fp_info.no_ddi_entries`, even though all in-tree dissectors that populate the structure had already been fixed to cap the value. The MR explicitly describes the change as checking in the consumer as well, and Anders Broman approved and merged it. Release-branch backports !26546 and !26547 carry the same hardening into supported branches.

**Implementation rule:** when a consumer receives a count-plus-fixed-array structure and will index the array using that count, validate `count <= capacity` at the consuming boundary before iteration or indexing. Producer-side validation is valuable but does not replace the consumer's own memory-safety check, especially for shared structures with multiple producers.

**Confidence:** Very high. Merged master hardening authored by John Thacker, approved by Anders Broman, with two accepted stable-branch backports.