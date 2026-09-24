# Wireshark Runtime State Consistency Conventions

This file records durable rules for keeping configuration and derived runtime state consistent. Current upstream source remains authoritative.

## Gate optional processing on both requested configuration and realized state

A current preference value does not prove that already-created conversation/file state was initialized for that preference. GUI-driven preference changes and redissection can temporarily expose old derived state together with new configuration. Code should not run an optional path unless both the requested setting and the concrete state/resources required by that path are present.

Merged master MR !10690, authored and merged by John Thacker, fixes TCP out-of-order reassembly when the preference is enabled after an existing TCP analysis object was created without its out-of-order segment lists. The accepted path requires both the preference and the realized per-flow list before attempting OOO reassembly; release-4.0 backport !10691 preserves the fix. Closed draft !10682 had explored synthesizing the missing list after the fact, but that direction was abandoned in favor of refusing to use a capability whose derived state had never been initialized.

**Architecture rule:** treat configuration and realized runtime state as separate facts. If a feature's persistent state is created conditionally, its consumers must either participate in a defined rebuild/invalidation transition or verify that the matching state exists before use. Do not infer old-object capabilities solely from the current preference value.

**Confidence:** Very high. Merged master lifecycle fix authored and merged by John Thacker, with the alternative state-synthesis proposal closed.
