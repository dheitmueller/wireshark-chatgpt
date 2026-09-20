# Wireshark Scripting API Compatibility Conventions

This file records durable compatibility conventions for APIs exposed to Lua and other interpreted extension mechanisms. Current upstream source and API documentation remain authoritative.

## Preserve deprecated aliases when renaming script-visible API identifiers

A source-level rename that is harmless for recompiled C/C++ callers can break user scripts immediately. Script-visible identifiers therefore need compatibility treatment based on how scripts are actually deployed, not merely on whether the corresponding native source identifiers have been renamed.

Merged master MR !15813, authored by John Thacker and approved/merged by Anders Broman, renames Telephony statistics group identifiers so their names match their current top-level menu location. The same identifiers are exposed to Lua, but the MR deliberately keeps the older Lua names as deprecated aliases because Lua scripts are commonly reused across Wireshark versions without recompilation.

**Implementation rule:** when renaming an enum, constant, registration group, or other identifier that is exposed through Lua or another interpreted API, preserve the old script-visible spelling as a deprecated alias when practical. Migrate internal/native users to the corrected name while keeping existing scripts functional across supported versions.

**Review rule:** treat bindings and scripts as independent API consumers. A rename audit should search not only native call sites but also exported Lua names, generated bindings, documentation examples, plugins, and other runtime extension interfaces whose users do not automatically rebuild against the new source tree.

**Confidence:** Very high. Merged master compatibility change authored by John Thacker and approved/merged by Anders Broman, with the cross-version Lua-script rationale stated explicitly in the MR description.
