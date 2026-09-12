# Wireshark Heuristic Reassembly Conventions

This file records durable conventions for the interaction between heuristic dissector selection and reassembly. Current upstream APIs and dissectors remain authoritative.

## Preserve the selected heuristic dissector across reassembly

When a fragment or initial payload has already been classified by a heuristic dissector, that classification is part of the protocol state needed to complete reassembly. Do not later let a different heuristic independently claim the reassembled payload merely because it also happens to match the bytes.

Merged MR !22720 fixes InfiniBand RC_SEND reassembly by retaining the dissector selected by `dissector_try_heuristic()` and using that same dissector when the message is reassembled. Without that remembered selection, SMB Direct traffic could subsequently be handed to RPC-over-RDMA reassembly even after `packet_is_rpcordma()` had rejected the original payload. Anders Broman approved and merged the fix.

**Implementation rule:** if heuristic dispatch participates in a fragmented/reassembled protocol path, store the selected dissector identity with the reassembly/conversation state and replay that decision when dissecting the completed unit. Re-running a set of competing heuristics at completion can change protocol identity between fragments and the reassembled message.

**Confidence:** Very high. Merged master correctness fix with a concrete cross-protocol misclassification failure mode and maintainer approval.
