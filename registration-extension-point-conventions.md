# Wireshark Registration Extension-Point Conventions

This file records durable conventions for protocol extension-point registration. Current upstream source remains authoritative.

## Create extension-point registries before the handoff phase that consumes them

Dissector tables, taps, and similar registries are identities that other protocols may consume during their handoff functions. Creating such an extension point in the owning protocol's handoff makes correctness depend on relative handoff order: another dissector can legitimately attempt to attach before the table exists.

Merged master MR !15568 moves the TZSP encapsulation dissector-table registration from `proto_reg_handoff_tzsp()` into `proto_register_tzsp()`. Its stated purpose is to guarantee that the table already exists for every other dissector that may hook into it from its own handoff. John Thacker approved and merged the change. Independently, merged master MR !16252 moves tap registrations from handoff into protocol registration across multiple dissectors for the same cross-dissector ordering reason.

**Architecture rule:** establish extension-point identities in protocol registration when peer handoff routines are expected to bind to them. Reserve handoff for installing bindings, resolving already-registered dependencies, and applying preferences; do not create a registry there if another handoff may need the registry first.

**Review rule:** when a new dissector table, tap, or equivalent registry is introduced, search for both producers and consumers. If consumption can occur from a different protocol's handoff, registration order must not depend on the incidental ordering of handoff callbacks. Generated/template sources must preserve the same lifecycle placement.

**Confidence:** Extremely high. The TZSP fix is a merged master change explicitly justified by cross-dissector availability and approved/merged by John Thacker, and the same lifecycle rule is independently demonstrated by the broader accepted tap-registration cleanup in !16252.
