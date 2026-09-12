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