# Wireshark Runtime State Consistency Conventions

This file records durable rules for keeping configuration and derived runtime state consistent. Current upstream source remains authoritative.

## Gate optional processing on both requested configuration and realized state

A current preference value does not prove that already-created conversation/file state was initialized for that preference. GUI-driven preference changes and redissection can temporarily expose old derived state together with new configuration. Code should not run an optional path unless both the requested setting and the concrete state/resources required by that path are present.

Merged master MR !10690, authored and merged by John Thacker, fixes TCP out-of-order reassembly when the preference is enabled after an existing TCP analysis object was created without its out-of-order segment lists. The accepted path requires both the preference and the realized per-flow list before attempting OOO reassembly; release-4.0 backport !10691 preserves the fix. Closed draft !10682 had explored synthesizing the missing list after the fact, but that direction was abandoned in favor of refusing to use a capability whose derived state had never been initialized.

**Architecture rule:** treat configuration and realized runtime state as separate facts. If a feature's persistent state is created conditionally, its consumers must either participate in a defined rebuild/invalidation transition or verify that the matching state exists before use. Do not infer old-object capabilities solely from the current preference value.

**Confidence:** Very high. Merged master lifecycle fix authored and merged by John Thacker, with the alternative state-synthesis proposal closed.

## Do not erase packet-final state that post-dissection consumers still query

State cleanup at a nested dissector boundary is not automatically correct merely because the state was established by the callee. Some `packet_info` fields intentionally describe the final interpretation of the packet and are queried by GUI or other post-dissection consumers after the last PDU has completed.

Merged master MR !9914, authored by John Thacker, reverts resetting the current conversation elements after every dissector call. The attempted cleanup was useful between peer PDUs, but `find_conversation_pinfo()` and other consumers need the final conversation/address state after the last PDU. The MR explicitly says the correct general boundary is closer to starting a new PDU at the same protocol level, and reverts the per-call save/restore until that lifecycle can be modeled correctly.

**Architecture rule:** define state reset boundaries from the lifetime of the semantic value, not from call-stack symmetry. Before restoring `packet_info` or similar shared context on function return, identify whether later packet-level consumers rely on the callee's final value. If state must be reset between logical PDUs, reset it at that PDU boundary rather than indiscriminately after every nested call.

**Confidence:** Very high. Merged master revert authored by John Thacker with the post-dissection consumer and desired lifecycle boundary stated explicitly.

