# Wireshark Initialization and Registration Lifecycle Conventions

This file records durable initialization-order conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Initialize subsystem registries at their owning lifecycle boundary, not lazily in registration calls

When a subsystem has a defined initialization phase, create its registry/container state there before registration begins. Registration functions should normally be able to rely on the subsystem having been initialized rather than repeatedly testing whether backing tables, arrays, or lists happen to exist and allocating them on first use.

Merged master MR !22249, authored and merged by Michael Mann, adds explicit initialization for conversation tables, Export Objects, Follow Stream, RTD, and SRT registries so their registration paths no longer have to test whether backing storage has been created. Merged master MR !22262 applies the same pattern to statistics trees and stat taps: `epan_init()` invokes `stats_tree_init()` and `stat_tap_init()`, and the registration routines stop lazily allocating their registries. Merged master MR !22263 likewise creates the postdissector array in `packet_init()` rather than on the first call to `register_postdissector()`.

**Architecture rule:** if registration is only valid after a subsystem's normal initialization entry point, allocate the subsystem's registration state in that entry point. Prefer explicit lifecycle invariants over hidden first-call initialization in individual registration APIs; this makes initialization order visible, removes repeated defensive checks, and keeps registration focused on registration.

**Confidence:** Very high. Three adjacent merged master refactors authored and merged by Michael Mann apply the same lifecycle pattern independently across multiple epan facilities.

## Register contributors before loading configuration that depends on their registrations

Initialization order also matters between distinct subsystem phases. If listeners, plugins, codecs, taps, or other registrants can themselves define UATs, preferences, or other configuration objects, those registrations must exist before the configuration-loading phase that discovers or populates them. Loading configuration first can silently omit registrant-owned state and cause later crashes when code assumes that state was initialized.

Merged master MR !22220, authored by Michael Mann and approved/merged by John Thacker, moves `register_all_tap_listeners()` into `epan_init()` and passes the application-specific listener list through `epan_app_data_t`. The motivating crash involved the Packet Length stat tap: tap listeners may own UATs, but UAT data had begun loading before tap listeners were registered, so that UAT was missed. Michael explicitly described this as an order-of-operations problem; John called the change a logical improvement and approved it. The change follows merged !22196, which passes application-owned initialization data into `epan_init()` instead of making epan query a global application-flavor policy.

**Architecture rule:** map the dependency graph of initialization phases, not just the lifetime of individual containers. Registration of components that contribute configuration/schema must precede the phase that loads or resolves that configuration. Prefer making that ordering explicit in the owning initialization API rather than relying on application call-site folklore.

**Review implication:** when moving an initialization phase, audit what that phase registers and what later/earlier phases consume. UATs and preferences are especially important because a change can appear harmless until a feature whose registration contributes configuration is exercised.

**Confidence:** Very high. Merged master fix for a concrete crash, authored by Michael Mann and explicitly endorsed by John Thacker; the failure mechanism and intended lifecycle boundary are described directly in the review discussion.
