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

## Establish mode selectors before initialization that derives state from them

If an initializer computes paths, plugin locations, configuration names, or other state from a process/application mode, that mode must be established before the initializer runs. Setting the selector afterward does not retroactively repair derived state that has already been cached or consumed.

Merged master MR !21100 fixes Stratoshark plugin discovery by calling `set_application_flavor(APPLICATION_FLAVOR_STRATOSHARK)` before `configuration_init()`. The earlier ordering initialized configuration while the process still appeared to be Wireshark, so flavor-dependent directories were wrong. The same correction was accepted on release-4.6 in !21114.

**Implementation rule:** identify every input that an initialization routine reads directly or indirectly and make those inputs valid first. In particular, establish application/product flavor, profile/environment selection, and equivalent policy selectors before initializing subsystems that derive filesystem paths or persistent configuration from them.

**Review implication:** when moving initialization calls or introducing a new mode selector, audit whether any earlier initializer snapshots or derives state from that selector. A later assignment can look locally correct while leaving previously derived configuration permanently stale for that process.

**Confidence:** Very high. Merged master correctness fix plus stable backport, with the failure directly observable as missing flavor-specific plugin discovery.

## Match resource lifetime to the state it actually depends on

Wireshark's lifecycle hooks are not interchangeable. `init` and `cleanup` routines participate in capture-file/preference lifecycle, while shutdown routines run once when the epan lifetime is ending. Resources whose contents are fixed and independent of the current capture or preferences should therefore be created once at registration/epan lifetime and destroyed at shutdown, rather than rebuilt on every capture or preference change.

**Evidence:** merged master MR !20715 changes the XML encoding `GRegex` from init/cleanup lifetime to registration/shutdown because it is fixed and independent of file/preferences. Merged master MR !20729, authored by John Thacker, applies the same rule to the logcat-text compiled regular expressions. Both changes explicitly distinguish capture/preference lifecycle from process/epan shutdown lifecycle.

**Review rule:** when reviewing an init/cleanup callback, ask what input can actually invalidate the resource. If neither capture state nor preferences affect it, repeated per-capture reconstruction is usually the wrong lifetime.

## Immutable lookup structures should not be rebuilt per dissection or per file

Tables derived entirely from constant generated data belong at a lifetime that matches that constant data. Construct them once in epan scope, and when their size is known, reserve appropriate capacity before bulk insertion. For very large generated constant mappings, also consider whether the generator should emit a sorted table/binary search structure or a perfect hash rather than constructing a dynamic map at runtime.

**Evidence:** John Thacker's merged master MR !20728 moves DICOM constant lookup maps from file scope/repeated initialization to one-time epan-scope creation. Merged follow-up !20730 reserves the exact generated table sizes before inserting thousands of entries and explicitly notes that generated binary/perfect-hash lookup may be a better long-term representation.

## Defer configuration-dependent side effects until effective preferences exist

Startup callbacks can run before profile preferences and command-line overrides have reached their effective values. A callback that is invoked early for structural reasons (for example, UAT loading) must not launch an expensive helper or otherwise commit configuration-dependent behavior based on defaults that may shortly be replaced.

Move the side effect to the preference-application phase or another point where the effective configuration is authoritative. If the desired behavior must occur even when a preference retains its default value, ensure the relevant apply path is still invoked; do not rely only on a detected value change.

**Evidence:** merged master MR !20725, authored by John Thacker, prevents `mmdbresolve` from being started by the first MaxMindDB UAT post-update callback because that callback occurs before all preferences and command-line options are read. The accepted change waits for name-resolution preference application and explicitly marks that module for application so the default-enabled case is handled too.

## Deferred or lazy registration must preserve the observable registration contract

Deferring expensive registration work can improve startup time, but laziness is an implementation detail and must not change what generic consumers observe. Any code that enumerates fields, prefixes, protocols, handoffs, or other registration products must either run after registration is complete or explicitly force completion before it queries the registry.

Merged !20665 experimented with deferring XML field registration while replacing hand-written parsing with libxml2. Merged successor !20684 deliberately restored much of the established XML registration/code flow while retaining the parser-backend improvement, so the accepted end state prioritized behavioral equivalence over keeping every part of the more aggressive lazy-registration refactor. Independently, merged !20674, authored by John Thacker, fixes the Qt supported-protocols loader by explicitly calling `proto_initialize_all_prefixes()` before iterating registered protocol fields; otherwise deferred fields could be missing from that consumer's view.

**Architecture rule:** optimize registration only behind a stable externally observable contract. If a registry supports deferred construction, provide a defined completion point and invoke it before generic enumeration/query APIs that require a complete view. During parser/backend migrations, first preserve protocol registration, handoff, field naming, and discovery behavior; separate additional lifecycle/performance changes when they would make equivalence harder to establish.

**Review implication:** when introducing lazy registration, search beyond the primary dissector path for consumers that enumerate registry state during startup, UI setup, taps, export, or command-line initialization. A fast startup path is not correct if another supported consumer sees an incomplete registry.

**Confidence:** Very high. The evidence is a sequence of merged master changes, including a later accepted correction of the more aggressive XML refactor and an independent John Thacker fix for a consumer that needed explicit deferred-registration completion.

Merged master MR !20495, authored by John Thacker, provides additional performance evidence for this rule: X11, ASTERIX, and ERF defer thousands of same-prefix field registrations until their common dissection entry point, reducing empty-capture TShark startup time by a little over 10% in the author's callgrind/cachegrind and real-world measurements. The optimization was practical precisely because those protocols had controlled entry points; protocols with configuration-driven registration or many entry points were explicitly called out as less suitable for the same treatment.

## Register a dissector handle only after its owning protocol has been registered

A dissector handle that is associated with a protocol ID must be created in a registration phase where that protocol ID is already valid. Do not register a handle for a sibling/sub-protocol from another protocol's registration function merely because both live in the same source file; doing so can bind the handle to protocol ID 0 or another not-yet-established value.

Merged master MR !20563, authored and merged by John Thacker, moves the DLT storage dissector handle from `proto_register_dlt()` into `proto_register_dlt_storage_header()` immediately after the storage-header protocol itself is registered. Stable backports !20565 and !20566 carry the same ordering fix.

**Implementation rule:** treat protocol registration as a dependency for any handle whose metadata references that protocol. When a file registers multiple protocols, keep each handle registration with the registration function that owns its `proto_*` ID, unless a later phase explicitly guarantees all required IDs are initialized.

**Confidence:** Extremely high. Merged John Thacker master fix with stable-branch propagation and an explicit failure mode in the MR description.

## Resolve stable registry identities in handoff, not in the packet hot path

If a dissector repeatedly looks up another already-registered protocol by a stable filter/name key, resolve that identity once in the handoff/registration lifecycle and retain the resulting protocol ID. Name-to-ID lookup is registry work, not packet-specific work.

Merged master MR !20530, authored and merged by Michael Mann, moves repeated `proto_get_id_by_filter_name()` calls out of multiple dissectors' packet paths and into their handoff functions, caching the resulting protocol IDs for `p_get_proto_data()` and similar use.

**Implementation rule:** use handoff as the phase for resolving stable cross-protocol handles/IDs after registration dependencies exist. Keep the dissection hot path focused on packet-dependent work; do not repeatedly hash or search immutable registry names for every packet.

**Review implication:** this optimization depends on lifecycle correctness. Before caching a protocol identity in handoff, verify that the target protocol has already been registered and that the identity cannot change with preferences or per-capture state.

**Confidence:** Very high. Merged master refactor by Michael Mann across multiple dissectors, with the performance rationale stated directly in the MR description.

## Parse control options at the earliest phase required by the subsystem they control

The general rule to defer preference-dependent side effects does not mean every command-line control belongs in the normal preference phase. If a subsystem can produce observable behavior before preferences are loaded, options controlling that early behavior must be interpreted before the first such use, by the subsystem that owns it.

Merged master MR !20534, authored and merged by Michael Mann, restores handling of legacy `-o console.log.level:...` inside `ws_log_parse_args()` because logging can occur before ordinary preferences are processed. The later preferences path recognizes the option but no longer owns the side effect.

**Implementation rule:** place configuration handling according to first-use dependency. Early bootstrap controls such as logging level may require subsystem-level argument parsing; ordinary behavior that depends on final profile/preferences should still wait for preference application. Avoid executing the same control independently in both phases.

**Confidence:** Very high. The merged fix explicitly identifies pre-preference logging as the reason the option must remain in `ws_log`'s early argument handling.
