# Wireshark Test-Prerequisite Conventions

Merged master MR !7926, authored by John Thacker, uses CMake/CTest fixtures so running the CTest unit suite or the build system's test target first builds `test-programs`. Release-4.0 MR !7941, authored by Guy Harris, carries the same behavior.

The change deliberately does not make direct pytest invocation build binaries; that remains a distinct entry point with different assumptions.

**Rule:** express prerequisites in the orchestration layer that owns the test command. A supported CTest/build-system workflow should build the helper executables it requires instead of depending on undocumented manual target ordering.
