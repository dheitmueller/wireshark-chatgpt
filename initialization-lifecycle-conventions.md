# Wireshark Initialization and Registration Lifecycle Conventions

This file records durable initialization-order conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Initialize subsystem registries at their owning lifecycle boundary, not lazily in registration calls

When a subsystem has a defined initialization phase, create its registry/container state there before registration begins. Registration functions should normally be able to rely on the subsystem having been initialized rather than repeatedly testing whether backing tables, arrays, or lists happen to exist and allocating them on first use.

Merged master MR !22249, authored and merged by Michael Mann, adds explicit initialization for conversation tables, Export Objects, Follow Stream, RTD, and SRT registries so their registration paths no longer have to test whether backing storage has been created. Merged master MR !22262 applies the same pattern to statistics trees and stat taps: `epan_init()` invokes `stats_tree_init()` and `stat_tap_init()`, and the registration routines stop lazily allocating their registries. Merged master MR !22263 likewise creates the postdissector array in `packet_init()` rather than on the first call to `register_postdissector()`.

**Architecture rule:** if registration is only valid after a subsystem's normal initialization entry point, allocate the subsystem's registration state in that entry point. Prefer explicit lifecycle invariants over hidden first-call initialization in individual registration APIs; this makes initialization order visible, removes repeated defensive checks, and keeps registration focused on registration.

**Confidence:** Very high. Three adjacent merged master refactors authored and merged by Michael Mann apply the same lifecycle pattern independently across multiple epan facilities.
