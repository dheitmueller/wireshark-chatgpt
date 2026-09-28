# Wireshark Specialized-Target CI Conventions

## A non-default target still needs deliberate build coverage

Merged MR !6407, authored and merged by Gerald Combs, changes fuzzshark from a platform-dependent default to BUILD_fuzzshark=OFF by default everywhere. The GitLab extra-warnings job explicitly configures BUILD_fuzzshark=ON so the target continues to compile in CI.

**Rule:** when a costly, specialized, or developer-only target is disabled in the normal default configuration, explicitly select one or more CI jobs that build it. Default-off must not become untested-by-default.

**Review rule:** whenever changing a CMake option default, search CI and packaging configurations for the target and verify that intended coverage remains explicit.

**Confidence:** Very high. Merged build/CI change authored by Gerald Combs.
