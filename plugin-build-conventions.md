# Wireshark Plugin Build Conventions

This file records durable conventions for Wireshark plugin build definitions extracted from accepted upstream changes. Current upstream CMake remains authoritative.

## Factor repeated plugin boilerplate by semantic plugin type, while preserving the generic compatibility primitive

When several plugin families share stable but type-specific CMake structure, encode that structure in a semantic helper for each plugin type rather than repeating the same target/link/install boilerplate in every plugin directory. Keep the lower-level generic helper available when it is part of the external/third-party build contract or when uncommon plugin types do not yet justify a dedicated abstraction.

Merged MR !24014, authored and merged by Michael Mann, introduces `add_wireshark_epan_plugin_library()`, `add_wireshark_tap_plugin_library()`, `add_wireshark_codec_plugin_library()`, and `add_wireshark_wiretap_plugin_library()` and migrates the corresponding in-tree plugins to those helpers. Display-filter and UI plugins remain on the generic `add_wireshark_plugin_library()` because there is only one of each, and the generic helper is deliberately retained for backward compatibility with third-party plugins.

**Implementation rule:** abstract build repetition at the level of the semantic component family that actually shares a contract. Do not force rare or external plugin types through a specialization that does not model them, and do not remove a generic build primitive when third-party consumers still depend on it merely because in-tree targets have moved to stronger wrappers.

**Confidence:** Very high. Merged master build-system refactor authored and merged by Michael Mann and applied across the existing plugin families.

## Follow the documented plugin target contract rather than copying another plugin blindly

Plugin `CMakeLists.txt` files should express their own source, support-source, plugin-library, and link requirements explicitly. Use current Wireshark build documentation and CMake helpers as the contract instead of cargo-culting a nearby plugin whose dependencies or historical boilerplate may differ.

Merged documentation MR !23863 replaces Developer's Guide guidance that effectively told contributors to copy the `gryphon` plugin with an explicit description of the required plugin CMake structure. It documents the plugin source/support lists, `add_wireshark_plugin_library(...)`, and `target_link_libraries(...)`; for an epan plugin, `epan` is the minimum link dependency, with additional libraries such as gcrypt or libxml2 added through their appropriate CMake variables when actually used. Michael Mann also pushed the discussion toward fixing questionable boilerplate rather than documenting it as a permanent pattern.

**Implementation/review rule:** when creating or updating a plugin, start from the semantic requirements documented by the current build system, then add only dependencies the plugin uses. Treat copied boilerplate as suspect: if it is not understood or required by the documented target contract, verify it before carrying it forward.

**Confidence:** High. Merged Developer's Guide update accepted by Anders Broman, with substantive review from Michael Mann, and it documents the project's intended build idioms directly.
