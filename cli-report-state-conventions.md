# Wireshark CLI Report State Conventions

This file records durable rules for command-line reports whose output is defined in terms of Wireshark's effective runtime configuration. Current upstream source remains authoritative.

## Reports that claim to show current configuration must initialize that configuration first

A command-line report can run early in process startup for efficiency, but early dispatch must not silently change the meaning of the report. If a report distinguishes the current profile's enabled/disabled state from built-in defaults, the preferences and profile state required to compute that distinction must be loaded before the report is generated.

Merged master MR !13750, authored by John Thacker, fixes `tshark -G protocols`, `-G heuristic-decodes`, and `-G decodes`. Those reports describe the current configuration, but the early `-G` dispatch occurred before the normal `epan_load_settings()` call, so profile changes were ignored and defaults were printed. The accepted fix explicitly loads settings for those reports while documenting that other early reports already receive their data through earlier initialization paths.

**Implementation rule:** define the semantic initialization prerequisites of each report independently of where its option is parsed. An early-exit/report path must explicitly perform every initialization step required by the data contract it advertises.

**Review rule:** do not assume that a report invoked after `epan_init()` has all profile-dependent state. Trace the startup order for each registry, preferences file, resolver database, plugin source, or other configuration input used by the report.

**Testing rule:** test current-configuration reports with a non-default profile value that changes their output. Compare the report against both defaults and the selected profile so a startup-order regression cannot pass by coincidence.

**Confidence:** Very high. Merged master correctness fix authored by John Thacker; the mismatch between report semantics and startup ordering is stated directly in the MR and encoded explicitly in the accepted change.
