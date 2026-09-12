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