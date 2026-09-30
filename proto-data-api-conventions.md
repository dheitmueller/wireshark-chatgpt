# Wireshark Proto-Data API Conventions

This file records durable conventions for packet/file-scoped protocol data and the APIs used to manage it. Current upstream source remains authoritative.

## Use scoped proto-data for shared dissection state, and choose add versus set deliberately

Protocol state that must be shared between dissectors or retained for a file does not require a process-global variable. Wireshark's proto-data mechanism associates values with a protocol ID and key at either packet scope (`pinfo->pool`) or file scope (`wmem_file_scope()`).

Merged master MR !5640, authored by Gerald Combs, adds public `p_set_proto_data()`. It searches for an existing entry with the same scope, protocol ID, and key; if found, it replaces the stored value, otherwise it delegates to `p_add_proto_data()`. The same MR switches protocol-depth tracking from unconditional add semantics to set semantics, documents the proto-data API family, and records the new exported symbol.

**State rule:** use proto-data instead of globals when the state naturally belongs to a packet or capture file. Pick the scope from the lifetime of the stored value and every object reachable from it.

**API rule:** use `p_add_proto_data()` when multiple entries with the same semantic identity are intentional; use `p_set_proto_data()` when (protocol,key) identifies one logical slot that should be replaced on update. Do not emulate replacement by repeatedly adding entries and relying on lookup order.

**API-evolution rule:** a new public libwireshark API should be documented at declaration and reflected in exported-symbol metadata in the same change.

**Confidence:** Very high. Merged master core-API change authored by Gerald Combs.


## Check actual proto-data presence instead of inferring it from the visited flag

Maintained-branch merged MRs !5162 and !5161 fix Gryphon crashes by looking up the dissector's per-packet proto-data first and creating it when it is absent. The old code assumed that `pinfo->fd->visited` meant Gryphon must have run on an earlier pass and therefore that its private packet state must already exist. Strange TCP sequence behavior can violate that assumption: a frame may be marked visited even though the nested dissector did not receive it on the nominal first pass.

**State rule:** before consuming dissector-owned packet/file proto-data, check for the actual state object. Treat `pinfo->fd->visited` as information about the frame's dissection lifecycle, not as proof that every nested dissector executed previously and initialized its own state.

**Recovery rule:** when creation is valid and idempotent for a missing state object, use state existence as the guard. This also makes redissection paths more robust to upstream reassembly/dispatch changes that alter when a nested dissector first sees a frame.

**Confidence:** High. Two merged maintained-branch fixes by Gerald Combs with a concrete segfault failure mode; the master-origin MR is outside this reviewed batch, so these are used as accepted corroborating evidence rather than claiming a master review here.


## Prefer actual dissector-owned proto-data existence over `visited` as the initialization predicate

Merged master MR !5153, authored by John Thacker, is the master origin of the Gryphon fix previously seen in maintained branches !5162 and !5161. Release-3.6 MR !5160 carries the same change. The dissector first asks `p_get_proto_data()` whether its packet state exists and creates it only when absent, instead of assuming that `pinfo->fd->visited` proves Gryphon must have initialized that state on an earlier pass. The bug was a real segfault triggered by unusual TCP sequence behavior in which the frame lifecycle and the nested dissector's execution history diverged.

**State rule:** a frame-level lifecycle flag is not a substitute for checking the state object a nested dissector actually owns. If later logic requires proto-data, test for that proto-data directly.

**Redissection rule:** when creation is valid and idempotent, use state presence as the initialization guard. This remains correct if reassembly or dispatch changes which pass first reaches the nested dissector.

**Confidence:** Extremely high. Merged master correctness fix by John Thacker, plus release-3.6, release-3.4, and master-3.2 propagation.

## Use native proto-data for native dissector packet state, not the Lua private table

Packet-scoped state owned by a native dissector belongs in Wireshark's proto-data mechanism rather than in pinfo->private_table, whose ownership/contract is for Lua.

Merged master MR !3855 replaces RTPS use of pinfo->private_table with p_add_proto_data() / p_get_proto_data(), keyed by the RTPS protocol ID and an explicit proto-data key. The participant GUID copied into that slot is allocated from pinfo->pool, so the state container and stored value have the same packet lifetime.

**API rule:** native dissectors should use the scoped proto-data APIs for per-packet cross-function/cross-dissector state. Do not repurpose a framework-private table owned by another API surface merely because it is reachable through packet_info.

**Lifetime rule:** allocate the stored value from a scope compatible with the proto-data entry, and use the same scope/protocol/key tuple for add and lookup.

**Confidence:** Very high. Merged master cleanup whose MR rationale explicitly identifies pinfo->private_table as Lua-only.
