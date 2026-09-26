# Wireshark Reassembly API Conventions

This file records durable conventions for using and evolving Wireshark's common reassembly machinery. Current upstream APIs remain authoritative.

## Keep fragment heads and fragment items as distinct semantic types

A reassembly record's aggregate/head metadata and its individual fragment nodes have different invariants and lifetimes. Do not treat them interchangeably merely because an older implementation happened to store both in one structure or expose overlapping members.

Merged master MR !8836, authored by John Thacker, split `fragment_head` and `fragment_item` into distinct structures, shrinking both and forcing callers to state which object they actually hold. The change exposed several callers that had depended on the old structural conflation. Clang Analyzer then found null-dereference paths in OBEX, SMB, SMB2, core reassembly, and OPC UA; merged follow-ups !8844, !8846, !8858, and !8859 corrected iterator-end, empty-list, and head/item assumptions.

**Implementation rule:** when traversing a reassembly record, treat the head as container/aggregate state and traverse fragment items through the head's fragment list. Do not cast or reuse one kind as the other, and make empty-list behavior explicit.

**Refactoring rule:** a type split that exposes previously implicit invariants is desirable, but it must be accompanied by a whole-tree audit of iterator termination, sentinel assumptions, first/last element handling, and empty-container cases. Run static analysis and the packet/fuzz regressions after the split; compilation alone is not evidence that old layout assumptions are gone.

**Confidence:** Very high. Merged core reassembly refactor by John Thacker plus four immediate merged correctness follow-ups and direct Clang Analyzer evidence.

## Trust the reassembly helper's returned TVBuff, not a frame-number coincidence

A frame can contain more than one layer or logical reassembly. Matching the current frame number to a reassembly record does not prove that the reassembled payload for the current dissector invocation is available.

Merged master MR !8847, authored by John Thacker, fixes PPP multilink traffic where PPP MP can occur at several layers in the same frame. The old code called the next dissector when `pinfo->num == frag_mp->reassembled_in`; the accepted code instead calls it only when `process_reassembled_data()` actually returns `next_tvb`. The MR explicitly notes that the helper checks both the packet and the correct protocol layer.

**Implementation rule:** after common reassembly processing, use the returned reassembled TVBuff (or equivalent helper success result) as the authority that payload is available for this invocation. Frame-number metadata is useful presentation/history state, but is not a substitute for layer-aware availability.

**Review rule:** test nested or multiplexed cases where more than one reassembly can complete in the same physical frame. A single-layer capture can hide incorrect frame-only tests.

**Confidence:** Extremely high. Merged master fix authored by John Thacker with the multi-layer failure mode stated directly.
