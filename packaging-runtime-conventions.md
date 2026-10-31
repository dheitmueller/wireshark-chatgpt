# Wireshark Packaging Runtime Conventions

This file records durable rules for behavior that depends on how an otherwise shared Wireshark binary is packaged or launched. Current upstream source remains authoritative.

## Detect packaging distinctions at the layer where they actually exist

A package variant is not necessarily a separately compiled program. If multiple packaging formats ship the same binary, compile-time macros cannot distinguish them unless the build genuinely produces separate binaries with different definitions.

Merged master MR !23609 originally attempted to disable software updates for PortableApps using a compile-time `PORTABLE` definition. John Thacker pointed out that PortableApps repackages the same compiled Wireshark binary used by other Windows package types, so that definition was never set and the proposed check could not work. The accepted revision instead detects the PortableApps launcher environment at runtime (`PORTABLEAPPS.com`), preserving updater behavior for normal installer packages. Guy Harris also participated in the revision history as the submission was reworked.

**Implementation rule:** before introducing a build-time conditional for packaging-specific behavior, verify that the packaging distinction actually exists at compile time. When the same executable is redistributed in multiple package forms, use an authoritative runtime/package signal supplied by that environment rather than inventing a compile-time distinction.

**Review rule:** trace the artifact pipeline from compilation through packaging and launch. A preprocessor check is only meaningful if the compiler invocation differs; packaging metadata or launcher-provided state belongs at packaging/runtime boundaries.

**Confidence:** Very high. Merged master behavior after direct John Thacker review corrected the original compile-time assumption, with further maintainer participation during the accepted revision.

## Relocatable bundles need coordinated runtime lookup overrides and privilege rules

Merged master !9504 (Gerald Combs) updates AppImage's generated AppRun wrapper to provide the bundled `LD_LIBRARY_PATH`, `WIRESHARK_DATA_DIR`, `WIRESHARK_EXTCAP_DIR`, and `WIRESHARK_PLUGIN_DIR` if the user has not already supplied overrides. It also documents those directory environment variables in the Wireshark and TShark manuals and their deliberate ineffectiveness in root/setuid execution. The change leaves a documented follow-up idea: deriving the resource directories relative to the executable path in `wsutil/filesystem.c` could reduce wrapper-specific configuration.

**Rule:** verify library, data, plugin, and extcap lookup *together* when moving to a relocatable distribution. Align build-time installed paths, launcher defaults, runtime lookup and user documentation; do not bypass privileged-process environment-variable restrictions to make the package work. **Confidence:** high for the merged AppImage implementation; executable-relative discovery is a suggestion, not implemented policy in this MR.
