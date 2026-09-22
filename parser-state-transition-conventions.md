# Wireshark Parser State-Transition Conventions

This file records durable conventions for parser helpers that consume protocol elements while carrying shared semantic state. Current upstream source remains authoritative.

## A parser helper that consumes a semantic unit should own the corresponding state transition

If a public/helper parser accepts the identity of the semantic unit it is consuming and successful parsing changes shared parser state, perform that state transition in the helper rather than requiring every caller to remember a matching assignment. Duplicating the transition at call sites is error-prone, especially when helpers can recurse into nested structures whose private state changes must not become the caller's outer-level state.

Merged master MR !13063 fixes this in the Thrift dissector. The `dissect_thrift_t_*()` helpers accept a `field_id`, but many of them did not update `thrift_opt->previous_field_id`. Callers therefore had to remember to do so manually. Missing updates could fail to detect genuinely decreasing field IDs, while a nested structure could leave its own final inner field ID in the shared state and make a later outer field appear spuriously unordered. The accepted implementation updates `previous_field_id` at the helper boundary after each successfully parsed field; list, set, map, and structure helpers retain the nested result, restore the outer semantic field identity, and then return. Release-4.2 backport !13080 carries the same correction and notes successful fuzzing.

**Implementation rule:** place state advancement at the narrowest API boundary that has both the semantic identity and proof of successful consumption. Callers should not need a parallel bookkeeping assignment merely because they invoked a parser helper.

**Nested-parser rule:** distinguish parent-level parser state from child-private state. After a nested helper returns successfully, restore/commit the state that represents the parent semantic unit before the caller continues. Do not let the final identifier or cursor state of a nested object leak outward unless the API explicitly defines that behavior.

**Review rule:** for parser option/state structures passed through several helper layers, list which fields each helper may read and mutate. Check all success paths, including compact/alternate encodings and nested-container paths, for consistent advancement; a helper family with one missing update can create false expert warnings far from the omission.

**Confidence:** Very high. Merged master correctness fix with an explicit worked failure example in the MR description, maintainer approval by Pascal Quantin, successful pipeline, and a merged stable-branch backport.