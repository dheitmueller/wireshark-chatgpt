# Wireshark Refactoring Conventions

This file records durable refactoring conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Consolidate duplicate and near-duplicate parser paths before they drift

When two read, seek, decode, or record-construction paths perform the same conceptual operation, prefer moving that operation into one shared helper or owner rather than maintaining parallel copies. Near-duplicate code is particularly risky because small differences can become accidental behavioral divergence and later fixes may be applied to only one copy.

Merged MR !21167 is unusually strong evidence. The log3gpp reader had two call sites that parsed a line and then independently assembled a packet record from the same collection of values. The accepted refactor moved packet-record construction into the parsing path and eliminated the duplicated declarations and call-site bookkeeping. Guy Harris explicitly endorsed the work, noting that reducing duplicated or almost-duplicated code is valuable because some differences between the copies may be unnecessary or buggy.

The same MR also models an important scope boundary: the author noticed questionable persistence semantics for `protocol.parameters` but deliberately did not change them because the file-format intent was not sufficiently understood. Behavior-preserving deduplication and semantic redesign were kept separate.

**Implementation rule:** if multiple parser paths implement the same operation, first make their intended behavior consistent and centralize the shared mechanism. Do not bundle an uncertain semantic change into that cleanup merely because the refactor exposes it; preserve behavior unless the protocol/file-format contract can be established and tested.

**Review rule:** treat differences between near-duplicate paths as suspicious until shown intentional. When reviewing a fix in one copy, search for sibling paths that perform the same operation and determine whether the fix should instead be made in shared code.

**Confidence:** Extremely high. Merged master refactor with direct, substantive Guy Harris review endorsing deduplication as correctness work rather than cosmetic cleanup.
