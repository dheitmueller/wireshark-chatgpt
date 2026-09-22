# Wireshark Configuration Precedence Conventions

This file records durable conventions for ordering configuration sources and derived runtime state. Current upstream source remains authoritative.

## Separate configuration-source selection, loaded preferences, preference overrides, derived options, and final invocation overrides

Startup configuration is not a single parse step. Some command-line arguments choose *which* configuration to read, some override preference values after that configuration has been loaded, preferences can then be projected into runtime/capture options, and other command-line arguments are explicit invocation-specific overrides that must win over those derived values.

Merged master MR !13285, authored and merged by John Thacker, fixes capture options that were being overwritten by preferences. The accepted startup sequence is explicit:

1. Parse command-line arguments that determine which preferences/configuration source to read, such as the profile.
2. Initialize capture options to defaults.
3. Read preferences.
4. Parse command-line arguments that override already-read preference values.
5. Apply those effective preferences to capture options.
6. Parse the remaining command-line arguments and set final capture-option values.
7. Apply other preferences.

The key semantic point is that persisted preferences are defaults for an invocation, while explicit capture arguments such as `-p`, `-n`/`-P`, `-H`, `-S`, and `--update-interval` are higher-precedence invocation overrides. The implementation therefore introduces a distinct `commandline_override_prefs()` phase and moves `prefs_to_capture_opts()` ahead of the final capture-option argument pass. Merged release-4.2 MR !13300 carries the same ordering fix.

**Architecture rule:** model configuration as ordered sources with dependencies, not as one mutable struct filled in arbitrary call order. Parse selectors before loading the source they select; apply command-line preference overrides only after that source exists; derive runtime state from the effective preference layer; and apply direct invocation/runtime overrides last.

**Review implication:** when startup code is reordered, identify both precedence and derivation edges. A value can be parsed from the right source and still be wrong if a lower-precedence layer is projected into runtime state afterward and silently overwrites it.

**Confidence:** Extremely high. Merged master fix authored and merged by John Thacker, with an explicit seven-stage ordering in the MR description and a merged stable-branch backport.