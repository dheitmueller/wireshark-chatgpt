# Wireshark Application-Layer Boundary Conventions

This file records durable dependency-direction and frontend-policy conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Keep application-specific policy behind the application layer

Values whose meaning depends on the running product or frontend should be resolved through the application layer rather than forcing generic lower layers to know whether they are serving Wireshark, Stratoshark, or another executable.

Merged MR !22987, authored and merged by Michael Mann, adds `application_extcap_dir()` so the appropriate extcap directory is selected through an application abstraction instead of scattered product checks. Merged MR !22991, also authored and merged by Michael Mann, moves VCS-version getter functions to the application layer for the same reason.

A useful boundary detail from !22991 is that `dumpcap` deliberately keeps its own version implementation rather than taking an upward dependency on the application layer merely to reuse that policy. Reuse does not justify reversing the architectural dependency direction.

**Architecture rule:** put frontend/product-specific path, version, resource, and behavior policy behind application-layer accessors. Keep standalone or lower-level executables independent of that layer when their architecture requires it, even if a small amount of implementation must remain local.

**Confidence:** Very high. Two adjacent merged master refactors authored and merged by Michael Mann with the abstraction goal stated directly.

## Shared low-level utilities must not depend upward on subsystem error definitions

Common utility code should expose errors in a domain it owns rather than including a higher subsystem header solely to borrow that subsystem's constants.

Merged MR !22943, authored by Michael Mann, approved and merged by Guy Harris, creates file-related error codes in `wsutil` so utility APIs no longer depend on `wtap.h` and Wiretap-specific error values. The MR notes that callers either use pass/fail semantics or pass the returned file error to `g_strerror()`, for which the borrowed `WTAP_` error domain was not appropriate anyway.

**Architecture rule:** maintain dependency direction from higher subsystems toward reusable lower layers. If a lower layer needs a small shared semantic domain such as generic file errors, define that domain at the lower/shared layer and translate at subsystem boundaries rather than importing a higher-layer header.

**Confidence:** Very high. Merged master layering cleanup explicitly approved and merged by Guy Harris.

## Let the application choose which protocol registration sets are installed

The dissection engine should provide mechanisms for registering protocols, but the choice of which registration groups belong to a particular executable is application policy. Keeping the protocol-registration inventory in a generic lower layer makes it harder for Wireshark, Stratoshark, and future frontends to compose different dissector sets without teaching the engine about each product.

Merged master MR !22636, authored and merged by Michael Mann, moves knowledge of the registration functions to the application layer. Applications supply registration and handoff callbacks to `epan_init()`, which passes them into `proto_init()` as part of generic engine initialization. This follows the same direction as the later application-layer path/version work: the generic engine executes registration, while the application decides what should be registered.

**Architecture rule:** separate registration mechanism from registration policy. Generic epan/proto initialization may invoke application-supplied callbacks, but product-specific knowledge of which protocol sets to load should remain above the reusable dissection engine.

**Confidence:** Very high. Merged master architectural refactor authored and merged by Michael Mann with the layering objective stated explicitly in the MR.

## Do not use packet-analysis taps merely as application lifecycle callbacks

A facility should be registered according to what it semantically is, not because an unrelated framework happens to provide a convenient callback at the desired time. UI/backend initialization belongs under application or frontend lifecycle control rather than being disguised as packet-analysis tap registration.

Merged master MR !22330, authored by Michael Mann, moves funnel operations outside the tap mechanism so applications explicitly control funnel-menu initialization. The adjacent merged !22333 reinforces the same boundary for CLI applications: funnel operations are application facilities, not packet taps. A competing ordering-only fix in !22329 was also merged, but !22330 is the more durable architectural direction because it removes the semantic mismatch rather than relying only on registration timing.

**Architecture rule:** use tap registration for packet-analysis listener semantics. When code only needs application initialization, menu/backend setup, or another lifecycle callback, expose and call an application-owned initialization mechanism instead of enrolling it as a tap solely for callback timing.

**Confidence:** Very high. The architectural refactor and follow-up are merged master changes authored by Michael Mann and explicitly describe funnel operations as not being taps.

## Keep frontends and generic extension layers independent of individual dissector implementations

Generic UI and extension infrastructure should not acquire a direct link-time dependency on a particular protocol dissector merely because that protocol needs specialized integration. Put the specialization behind a generic callback/registration hook, or keep a small adaptation in the consumer layer when that preserves the intended dependency direction.

Merged master MR !22245, authored and merged by Michael Mann, extends the generic `decode_as_t` abstraction with the specialized list-building operation needed by DCE/RPC so the UI no longer explicitly links to the DCE/RPC dissector. Merged master MR !22247 applies the same principle to WSLua: DCE/RPC handle adaptation is implemented locally in the Lua dissector layer instead of exporting and depending on a DCE/RPC-specific helper solely for that consumer. The closed illustrative MR !22275 was never intended to merge, but its stated purpose—checking that dissector code does not creep into the UI—corroborates the boundary while carrying less evidentiary weight than the two accepted refactors.

**Architecture rule:** protocol-specific behavior may plug into generic UI or scripting facilities, but generic frontends should depend on generic interfaces rather than concrete dissector implementations. Prefer callbacks, registries, or localized adapters over upward or sideways dependencies that make UI/extension infrastructure explicitly link to one protocol.

**Confidence:** Very high. Two adjacent merged master dependency-cleanup refactors authored and merged by Michael Mann; the separate illustrative no-dissector build provides weaker corroborating intent.

## Keep protocol-specific shared code with dissectors, not in generic epan

Code or generated data shared by several dissectors is not automatically generic dissection-engine infrastructure. If its semantics are specific to one protocol family, it should live with the dissectors rather than turning `epan/` into a catch-all shared-code directory.

In merged MR !22141, Michael Mann explicitly asked that MIDI SysEx shared generated data follow the `packet-bluetooth-data.c` pattern and stay under `epan/dissectors/`. He stated the architectural objective of isolating `epan/` as the generic dissection engine and the dissector directory as a separate protocol-specific library boundary. The final MR also simplified the generated-file layout after moving the data into the dissector directory.

The adjacent merged application-flavor refactors !22165, !22175, !22177, !22183, and !22188 strongly corroborate the same dependency direction: `wsutil`, Wiretap, epan internals, preferences, and dissectors should not reach upward into application-specific policy; that policy is pushed toward explicit application/epan boundaries instead.

**Architecture rule:** decide placement by semantic ownership, not merely by reuse count. Shared protocol-specific tables, helpers, and generated data belong with dissectors; reserve `epan/` for facilities that are genuinely part of the generic dissection engine. Likewise, generic lower libraries should receive application-specific policy through explicit interfaces rather than directly calling application-flavor APIs.

**Confidence:** Very high. The placement rule comes from explicit Michael Mann review on a merged MR and is reinforced by a sequence of merged layering refactors from the same maintainer.

## Pass owning application and operation context explicitly instead of rediscovering it in shared code

Once application-specific policy has been moved out of a shared layer, the replacement should normally be explicit data flow: the owner passes the product identity, options, or operation context that the lower layer actually needs. Replacing one hidden product probe with another hidden global leaves the same dependency problem under a different name.

Merged master MRs !22081 and !22082, authored by Michael Mann, move Stratoshark-specific Follow Stream and search behavior into application-specific subclasses instead of testing application flavor in common Qt classes. Merged !22083 passes the application name through the capture interface, and merged !22087 does the same for software-update initialization. Merged !22078 independently reduces use of `global_capture_opts` by passing the relevant capture-options object to the code performing the operation.

**Architecture rule:** make ownership visible in APIs. Product-specific behavior belongs in the owning application/subclass, while reusable code should receive only the application identity, options object, or operation context it needs through parameters or narrowly defined interfaces. Avoid application-flavor probes and mutable process globals as implicit dependency injection.

**Confidence:** Very high. A cluster of merged master refactors authored by Michael Mann all move hidden application/global context toward explicit ownership and parameter flow.

## Prefer link-time composition when application behavior is a whole implementation choice

When Wireshark and Stratoshark need different implementations of the same application-level facility, the distinction does not necessarily need to remain a runtime enum or `is_wireshark` / `is_stratoshark` branch. If each executable can select a complete implementation module, linking the appropriate module makes the dependency graph itself express the product choice and keeps lower shared libraries free of product tests.

Merged master MR !21941, authored and merged by Michael Mann, pulls the former `application flavor` implementation into separate application libraries for packet-oriented Wireshark and event-oriented Stratoshark. The MR's stated goal is that applications link the module they support rather than explicitly setting behavior, and its follow-up discussion describes continuing to bubble product checks up toward the UI/application layer. Guy Harris specifically reviewed whether command-line applications such as `tshark`, `strato`, and `tfshark` also consume the new abstraction, reinforcing that the composition boundary must cover every executable that depends on the facility rather than only the Qt frontends.

**Architecture rule:** where product identity selects an implementation rather than a small data value, consider module/library composition instead of runtime flavor checks. Make each executable link the implementation it owns, and audit GUI and command-line consumers together so the abstraction does not leave hidden product-specific branches in shared code.

**Confidence:** Very high. Merged master architectural refactor authored and merged by Michael Mann, with direct architecture-focused review from Guy Harris.