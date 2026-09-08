# Wireshark Build-System Conventions

This file records durable build-system conventions extracted from accepted upstream Wireshark changes. Current upstream build files remain authoritative.

## Model object libraries as targets and propagate their usage requirements

When a CMake object library is a reusable build component, link it as a target rather than manually injecting `$<TARGET_OBJECTS:...>` into each consumer's source list. Put the component's include paths and library requirements on the component target with the appropriate `PUBLIC` or `PRIVATE` scope so CMake can propagate them transitively.

Merged MR !26062, authored, approved, and merged by John Thacker, updates the extcap build this way now that Wireshark's minimum CMake version supports object libraries in `target_link_libraries()`. In particular, `ssh-base` owns its libssh include and link requirements as `PUBLIC`, because its public header exposes libssh types, while extcap executables link `cli_main`, `extcap-base`, and `ssh-base` as targets instead of splicing their object files into source lists.

**Implementation rule:** express build dependencies at the target that semantically owns them and let CMake usage requirements propagate. Avoid duplicating transitive include/library dependencies in every final executable merely because an object library is involved.

**Confidence:** Very high. Merged master build-system change authored and merged by John Thacker.